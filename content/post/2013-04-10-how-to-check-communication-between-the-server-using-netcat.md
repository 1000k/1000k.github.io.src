---
title: Netcat でサーバー間の疎通を確認する方法
author: 1000k
layout: post
date: 2013-04-10
url: /2013/04/10/how-to-check-communication-between-the-server-using-netcat/
categories:
  - その他
tags:
  - Linux
  - TIPS
---
サーバー間のネットワークが繋がっているか確認したい場合、Netcat (nc) コマンドが便利です。

<!--more-->

## 使うコマンド

受信側では "-l" (Listen mode) オプションを使い、待受状態にします。

```
$ nc -l {listenするポート番号}
```


送信側は "-z" (Zero-I/O mode) オプションを使い、指定したポートに接続できるかテストします。

```
$ nc -v -z {受信側のIP} {受信側のポート番号}
```


## 例

host001 のポート 4949 に、host002 から疎通できるかどうかテストするには、下記のようなコマンドを叩きます。

### 1. 受信側のポートを listen する

```
[host001]$ nc -v -k -l 4949
```


"-v" でより多くのメッセージを出力し、"-k" でコネクションを永続化することができます。（"-k" を指定しないと1回受信するたびに nc が終了します）

### 2. 送信側から接続できるか確かめる

```
[host002]$ nc -v -z host001 4949
```


接続 OK の場合、以下のようなメッセージが出力されます。

送信側

```
Connection to host001 4949 port [tcp/munin] succeeded!
```


受信側

```
Connection from host002 port 4949 [tcp/munin] accepted
```


## 参考

  * [Learn the many uses of netcat | TechRepublic](http://www.techrepublic.com/article/learn-the-many-uses-of-netcat/5689982)
  * [Netcat でネットワークをもう少し活用する - いますぐ実践! Linuxシステム管理 / Vol.190](http://www.usupi.org/sysad/190.html)