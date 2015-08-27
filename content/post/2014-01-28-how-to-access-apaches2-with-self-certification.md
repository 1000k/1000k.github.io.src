---
title: Apache2 にオレオレ証明書で HTTPS アクセスできるようにする手順
author: 1000k
layout: post
date: 2014-01-28
url: /2014/01/28/how-to-access-apaches2-with-self-certification/
categories:
  - Apache
tags:
  - Apache
  - TIPS
  - チュートリアル
---
よくやるけどすぐ忘れるのでメモ。

## 検証環境

  * CentOS 6.3
  * Apache 2.2.26
  * 設定ファイルの格納フォルダ: `/usr/local/apache2/conf`
  * httpd.conf に `Include conf.d/*.conf` を書き、`/usr/local/apache2/conf.d/*.conf` を自動ロードできるようにしてあります。

<!--more-->

## チュートリアル

オレオレ証明書を作る。(有効期限は10年)

```
$ cd /usr/local/apache2/conf/
$ sudo mkdir ssl.crt
$ sudo mkdir ssl.key
$ sudo sh -c "openssl genrsa 2048 > ssl.key/server.key"
$ sudo sh -c "openssl req -new -key ssl.key/server.key > server.csr"
(質問は全てデフォルトのまま Enter)
$ sudo sh -c "openssl x509 -days 3650 -req -signkey ssl.key/server.key < server.csr > ssl.crt/server.crt"
```


https アクセスを有効にする。

```
$ sudo sh -c "cat <<EOT > /usr/local/apache2/conf.d/ssl.conf
NameVirtualHost *:443
Listen 443

<VirtualHost *:443>
    DocumentRoot /var/www

    SSLEngine on
    SSLCertificateFile conf/ssl.crt/server.crt
    SSLCertificateKeyFile conf/ssl.key/server.key
</VirtualHost>
EOT"
```


Apache を再起動する。

```
$ sudo service httpd restart
```


ダン！

## 参考

  * [ubuntuにオレオレ証明書を入れてapacheにhttpsできるようにする話。 | smokycat.info](http://smokycat.info/ubuntu/440)
  * [オレオレ証明書をopensslで作る（詳細版） - ろば電子が詰まっている](http://d.hatena.ne.jp/ozuma/20130511/1368284304)