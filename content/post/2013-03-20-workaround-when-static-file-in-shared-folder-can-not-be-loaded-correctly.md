---
title: VM の共有フォルダ内の 静的ファイルが正しくロードできないときの対処方法
author: 1000k
layout: post
date: 2013-03-20
url: /2013/03/20/workaround-when-static-file-in-shared-folder-can-not-be-loaded-correctly/
categories:
  - Apache
  - 開発環境
tags:
  - Apache
  - Linux
  - PHP
  - VirtualBox
  - トラブルシューティング
---
以下の環境でPHPアプリの開発を行っていたところ、静的コンテンツが正しく反映されない現象に陥りました。

  * Oracle VirtualBox で CentOS 6.3 VM を起動
  * ホストOSは Windows 7
  * ホストOS側の "c:\www\app" を VM 側の "/www" に共有フォルダとしてマウント
  * VM上は Apache 2.2 を稼働させ、PHP アプリサーバーとして利用

この状態でホストOS側で JavaScript などの静的コンテンツを編集し、ブラウザからアプリにアクセスすると、以下のようなエラーが出ました。

Chrome

```
Uncaught SyntaxError: Unexpected end of input
または
Uncaught SyntaxError: Unexpected token ILLEGAL
```


Firefox

```
SyntaxError: unterminated string literal
```


ブラウザのデバッグツールで問題のファイルを見ると、なぜか途中でコンテンツが切れていたり、末尾に謎の文字列（Chromeだと「?」で表示される）が追加されていました。

ただ改行を追加するだけでもこの現象が発生します。

原因は Apache の _EnableSendfile_ ディレクティブでした。

<!--more-->

## 原因

[EnableSendfile ディレクティブ](http://httpd.apache.org/docs/2.2/ja/mod/core.html#enablesendfile) の説明には以下のように書いてあります。

> このディレクティブはクライアントにファイルの内容を送るときに httpd がカーネルの sendfile サポートを使うかどうかを制御します。デフォルトでは、 例えば静的なファイルの配送のように、リクエストの処理にファイルの 途中のデータのアクセスを必要としないときには、Apache は OS が サポートしていればファイルを読み込むことなく sendfile を使って ファイルの内容を送ります。
>
> ネットワークマウントされた DocumentRoot (例えば NFS や SMB) では、カーネルは自身のキャッシュを使ってネットワークからのファイルを 送ることができないことがあります。

つまり、VirtualBox で共有フォルダが NFS としてマウントされていると、ホストOSで行った変更が正しく反映されないことがあるようです。

## 直し方

_/etc/httpd/conf/httpd.conf_ に以下の1行を追加して、Apache を再起動すればOKです。

```
EnableSendfile Off
```


これで、ホストOSで変更したファイルが、ゲストOS側のキャッシュを介することなく、正しく反映されるようになります。

Vagrant + Chef で環境構築が楽にできるようになったから喜んで使っていたら、こんな罠があるとは。

## 補足: EnableMMAP ディレクティブ

私はまだ遭遇していないのですが、[<em>EnableMMAP</em> ディレクティブ](http://httpd.apache.org/docs/2.2/ja/mod/core.html#enablemmap)も Apache をクラッシュさせる危険があるので、Offにしておいた方が良いそうです。

> NFS マウントされた DocumentRoot では、httpd がメモリマップしている間にファイルが削除されたり 短くなったりしたときに起こるセグメンテーションフォールトのために httpd がクラッシュする可能性があります。

結局、httpd.conf に以下の2行を書いておけば不具合は避けられるでしょう。

```
EnableSendfile Off
EnableMMAP Off
```


## 参考

  * [DocumentRootに共有フォルダを使った場合の問題 - フリップフラップ](http://blog.flup.jp/2009/04/06/problem_of_using_shared_folder_to_document_root/)
  * [Virtualbox上のApacheでホストマシンと共有している静的ファイル（CSSなど）の更新が検知されない問題を解決する方法 | tipshare.info](http://tipshare.info/view/4f3481ee4b21227814000001)
  * [core - Apache HTTP サーバ](http://httpd.apache.org/docs/2.2/ja/mod/core.html#enablemmap)