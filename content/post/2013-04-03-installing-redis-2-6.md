---
title: Redis 2.6 のインストール手順
author: 1000k
layout: post
date: 2013-04-03
url: /2013/04/03/installing-redis-2-6/
categories:
  - その他
tags:
  - Redis
  - チュートリアル
---
なぜか<a href="http://redis.io/download" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://redis.io/download', '公式サイトのインストール手順']);" >公式サイトのインストール手順</a>はコンパイルまでで終わっているので、完全な手順をメモしておきます。

<!--more-->

## ダウンロード

最新のソースを <a href="http://redis.io/download" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://redis.io/download', 'Download – Redis']);" >Download – Redis</a> からダウンロードします。

以下、執筆時のバージョン (2.6.12) で話を進めます。

```
$ wget http://redis.googlecode.com/files/redis-2.6.12.tar.gz
```


## コンパイル

```
$ tar xzf redis-2.6.12.tar.gz
$ cd redis-2.6.12
$ make
```


※32bit版OSの場合、そのまま make するとエラーが出るので、以下のコマンドを実行します。

```
$ export CFLAGS=-march=i686
$ make distclean
$ make
```


参考: <a href="http://www.eschrade.com/page/undefined-reference-to-__sync_add_and_fetch_4/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.eschrade.com/page/undefined-reference-to-__sync_add_and_fetch_4/', 'undefined reference to `__sync_add_and_fetch_4′ when compiling Redis | ESchrade']);" >undefined reference to `__sync_add_and_fetch_4′ when compiling Redis | ESchrade</a>

## インストール

インストール後は対話式インストーラーを使って、各種ファイルを設置します。

基本的にはそのまま Enter でOKです。

```
$ sudo make install
$ cd utils
$ sudo ./install_server.sh

Welcome to the redis service installer
This script will help you easily set up a running redis server


Please select the redis port for this instance: [6379]
Selecting default: 6379
Please select the redis config file name [/etc/redis/6379.conf]
Selected default - /etc/redis/6379.conf
Please select the redis log file name [/var/log/redis_6379.log]
Selected default - /var/log/redis_6379.log
Please select the data directory for this instance [/var/lib/redis/6379]
Selected default - /var/lib/redis/6379
Please select the redis executable path [/usr/local/bin/redis-server]
s#^port [0-9]{4}$#port 6379#;s#^logfile .+$#logfile /var/log/redis_6379.log#;s#^dir .+$#dir /var/lib/redis/6379#;s#^pidfile .+$#pidfile /var/run/redis_6379.pid#;s#^daemonize no$#daemonize yes#;
Copied /tmp/6379.conf => /etc/init.d/redis_6379
Installing service...
Installation successful!
```


## 起動スクリプトの修正

ここまでで **/etc/init.d/redis_6379** に起動スクリプトが追加されますが、なぜか1行目のコメント行がバグっていて変数がすべてコメントアウトされています。

```
#/bin/sh\n #Configurations injected by install_server below....\n\n EXEC=/usr/local/bin/redis-server\n CLIEXEC=/usr/local/bin/redis-cli\n PIDFILE=/var/run/redis_6379.pid\n CONF="/etc/redis/6379.conf"\n\n REDISPORT="6379"\n\n ###############\n\n
```


なので、1行目の「\n」をすべて改行コードに置換します。修正後は以下のようになります。

```
#!sh
#/bin/sh
#Configurations injected by install_server below....

EXEC=/usr/local/bin/redis-server
CLIEXEC=/usr/local/bin/redis-cli
PIDFILE=/var/run/redis_6379.pid
CONF="/etc/redis/6379.conf"
REDISPORT="6379"
###############
```


vimなら「[ESC]キー」-> 「:%s/&#92;n/^M/g」を入力すれば一発です。（改行文字「^M」は「Ctrl+V Ctrl+M」で入力可能）

## 起動テスト

```
$ sudo /etc/init.d/redis_6379 start
$ ps -ef | grep redis
$ sudo /etc/init.d/redis_6379 stop
```


以上で完了です。