---
title: pacoでソースからインストールしたアプリを管理する
author: 1000k
layout: post
date: 2012-01-15
url: /2012/01/15/managing-source-installed-packages-using-paco/
categories:
  - Linux
  - コマンド
tags:
  - Linux
  - paco
---
pacoは、自分でソースからインストールしたアプリでもrpmのようにパッケージ管理できるようになるツールです。

rpmが無いアプリの管理が楽になるのでおすすめです。

以下、インストール方法と使い方を簡単に記します。

<!--more-->

## インストール手順

最新のソースは <a href="http://paco.sourceforge.net/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://paco.sourceforge.net/', 'paco &#8211; a source code pacKAGE oRGANIZER for Unix/Linux']);" >paco &#8211; a source code pacKAGE oRGANIZER for Unix/Linux</a> で確認してください。

```
# wget http://sourceforge.net/projects/paco/files/paco/2.0.9/paco-2.0.9.tar.bz2/download
# tar xvf paco-2.0.9.tar.bz2
# cd paco-2.0.9
# ./configure --disable-gpaco
# make
# make install
# make logme
```


今回はGUIを使わないので、configureオプションに「&#8211;disable-gpaco」を付けています。

これを付けないと「No package &#8216;gtkmm-2.4&#8217; found」というエラーが出ます。

最後の「make logme」でpaco自身のインストール内容を記録しています。

## 使い方

### インストールを記録する

今後、何らかのアプリをmake installする際には、「paco -D」を付けることでpacoに記録できます。

```
# cd {インストールしたいパッケージ}
# paco -D make install
```


### インストール済みパッケージを確認する

「-a」オプションを使います。

```
# paco -a
paco-2.0.9
```


### パッケージでインストールされたファイルを確認する

「-f」オプションを使います。

```
$ paco -f paco-2.0.9
paco-2.0.9:
/usr/local/share/paco/README
/usr/local/lib/libpaco-log.a
/usr/local/lib/libpaco-log.la
/usr/local/lib/libpaco-log.so
/usr/local/lib/libpaco-log.so.0
/usr/local/lib/libpaco-log.so.0.0.0
/usr/local/bin/paco
/usr/local/etc/pacorc
/usr/local/lib/pkgconfig/paco.pc
/usr/local/share/man/man5/pacorc.5
/usr/local/share/man/man8/paco.8
/usr/local/share/man/man8/pacoball.8
/usr/local/share/man/man8/rpm2paco.8
/usr/local/share/man/man8/superpaco.8
/usr/local/share/paco/faq.txt
/usr/local/share/paco/pacorc
/usr/local/bin/ocap
/usr/local/bin/pacoball
/usr/local/bin/rpm2paco
/usr/local/bin/superpaco
```


### パッケージをアンインストールする

「-r」オプションです。

```
# paco -r {アンインストールしたいパッケージ名}
```


## 参考

  * <a href="http://d.hatena.ne.jp/kasei_san/20100220/p1" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://d.hatena.ne.jp/kasei_san/20100220/p1', 'さくらのユーザディレクトリにPACOをインストールしたときの手順 &#8211; かせいさんとこ']);" >さくらのユーザディレクトリにPACOをインストールしたときの手順 &#8211; かせいさんとこ</a>
  * <a href="http://paco.sourceforge.net/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://paco.sourceforge.net/', 'paco &#8211; a source code pacKAGE oRGANIZER for Unix/Linux']);" >paco &#8211; a source code pacKAGE oRGANIZER for Unix/Linux</a>
  * <a href="http://www.atmarkit.co.jp/flinux/rensai/linuxtips/886usepaco.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.atmarkit.co.jp/flinux/rensai/linuxtips/886usepaco.html', 'ソースからインストールしたアプリを管理するには － ＠IT']);" >ソースからインストールしたアプリを管理するには － ＠IT</a>