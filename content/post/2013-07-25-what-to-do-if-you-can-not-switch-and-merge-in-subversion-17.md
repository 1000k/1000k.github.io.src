---
title: Subversion 1.7 で switch や merge ができないときの対処方法
author: 1000k
layout: post
date: 2013-07-25
url: /2013/07/25/what-to-do-if-you-can-not-switch-and-merge-in-subversion-17/
categories:
  - その他
tags:
  - CentOS
  - Subversion
  - トラブルシューティング
---
Subversion 1.7.10 をソースからインストールしたら、`svn switch` や `svn merge` が以下のエラーを吐いて失敗するようになってしまいました。

```
# svn merge https://uso.com/svn/mojamoja/2.13.1
...
svn: E200007: Error running context
svn: E200007: Trying to use an unsupported feature
```


直し方をメモしておきます。

<!--more-->

## 原因

Subversion は内部の HTTP/HTTPS クライアントとして serf または neon を使います。Configure オプションを特に指定しない場合は serf が使われるようですが、これに不具合があったようです。

何も configure オプションを指定せずにインストールした場合、`svn --version` を実行すると下記の通り出力されます。

```
# svn --version
svn, version 1.7.10 (r1485443)
   compiled Jul 24 2013, 16:16:57
...
* ra_serf : Module for accessing a repository via WebDAV protocol using serf.
  - handles 'http' scheme
  - handles 'https' scheme
```


「serf が http と https をハンドルするぜ」と書いていますが、実際には E200007 エラーを吐いて止まります。

## 対策

もう一つの選択肢である neon を使うようにしたら、問題なく動作するようになりました。

再コンパイル手順は以下のとおりです。

```
# cd subversion-1.7.10/

// 依存しているパッケージをダウンロードしてくれる便利なスクリプト。
# ./get-deps.sh

// neon をコンパイルしてインストールする。
// (CentOS なら `yum install neon neon-devel` でも OK)
// `--with-ssl` を忘れずに！
# cd neon/
# ./configure --with-ssl
# make &#038;&#038; make install

// Subversion をインストールする。
# cd ../
# make distclean
# ./configure \
  --without-berkeley-db \
  --with-neon \
  --with-ssl
# make &#038;&#038; make install
```


正しくインストールできると、以下のように `ra_neon` という表示が出ます。

```
# svn --version
...
* ra_neon : Module for accessing a repository via WebDAV protocol using Neon.
  - handles 'http' scheme
  - handles 'https' scheme
* ra_svn : Module for accessing a repository using the svn network protocol.
  - handles 'svn' scheme
* ra_local : Module for accessing a repository on local disk.
  - handles 'file' scheme
```


`handles 'https' scheme` と出力されない時は、`--with-ssl` を付け忘れています。それでは https 通信ができないので、再コンパイルしてください。

## 余談

今回とは逆で、「neon だとエラーになるが serf なら大丈夫だった」という事例もありました。

<a href="http://masutaka.net/chalow/2009-06-15-1.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://masutaka.net/chalow/2009-06-15-1.html', 'Debian squeeze の subversion で http リポジトリにアクセスできなくなった / マスタカの ChangeLog メモ']);" >Debian squeeze の subversion で http リポジトリにアクセスできなくなった / マスタカの ChangeLog メモ</a>

バージョンアップのたびに不具合が入れ替わるんでしょうか。

奇妙です。

## 参考

  * <a href="http://blog.iss.ms/2009/04/12/141537" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.iss.ms/2009/04/12/141537', '[memo][svn] svnクライアントにSSLをサポートさせる « いわぶろ（ろてん）']);" >[memo][svn] svnクライアントにSSLをサポートさせる « いわぶろ（ろてん）</a>
  * <a href="http://blog.r-unit.co.jp/archives/726" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.r-unit.co.jp/archives/726', 'はだかの隊長日記 » Subversionコンパイル方法']);" >はだかの隊長日記 » Subversionコンパイル方法</a>
  * <a href="http://masutaka.net/chalow/2009-06-15-1.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://masutaka.net/chalow/2009-06-15-1.html', 'Debian squeeze の subversion で http リポジトリにアクセスできなくなった / マスタカの ChangeLog メモ']);" >Debian squeeze の subversion で http リポジトリにアクセスできなくなった / マスタカの ChangeLog メモ</a>