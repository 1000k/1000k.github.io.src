---
title: php5enmodでPHP拡張を有効化する方法
author: 1000k
layout: post
date: 2012-10-28
url: /2012/10/28/enable-php-extensions-by-php5enmod/
categories:
  - PHP
tags:
  - MongoDB
  - PHP
  - チュートリアル
  - トラブルシューティング
---
Ubuntuにて、peclでMongoDBをインストールしてさあ動かそうと思ったら、以下のエラーが出て動きませんでした。

```
[Sun Oct 28 19:39:08 2012] [error] [client *.*.*.*] PHP Fatal error:  Class 'Mongo' not found in /var/www/html/*.php on line 38
```


php.iniも編集したし、Apache再起動もしたのになぜ？と軽くつまづきました。

けっきょく原因はUbuntuの作法を分かっていなかったが為のエラーでした。

<!--more-->

## 原因

UbuntuではApache2同様、PHPの設定ファイルも階層化されています。

知らないとはまるのが、WEB用の設定とCLI用の設定が異なることです。

  * WEB用: /etc/php5/apache2/php.ini
  * CLI用: /etc/php5/cli/php.ini

もしmongo.soを両方で使いたければ、両方の設定をしなければなりません。

私はCLI用のファイルしか設定していなかったので、WEBからアクセス時に Class not found エラーになっていました。

それは面倒だ、という場合の為にちゃんと抜け道が用意されています。

**php5enmod**コマンドを使ってmodを両方に有効化させることができます。

※php5enmod は **PHP >= 5.4.0** をパッケージでインストールした場合のみ有効です。

## 手順

mongo.soをWEB/CLI両方で有効にしたい場合は、下記の手順を踏みます。

  1. **/etc/php5/mods-available/mongo.ini**を作成する。

```
extension=mongo.so
```


  1. modを有効化する。

```
$ sudo php5enmod mongo
```


  1. Apache2を再起動する。

```
$ sudo service apache2 restart
```


  1. WEB/CLIで両方で設定が反映されているか確認する。

    CLIからは「php -i | grep mongo」を叩き、WEBからは phpinfo() を見て、それぞれ「mongo」という項目があればOKです。

うまくいかない場合、ログ（/var/log/apache2/error.log）を見てエラーメッセージを確認してください。

## 参考

  * <a href="https://bugs.launchpad.net/ubuntu/+source/php-apc/+bug/994882" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://bugs.launchpad.net/ubuntu/+source/php-apc/+bug/994882', 'Bug #994882 “php5{en/dis}mod not available in php 5.3.10” : Bugs : “php-apc” package : Ubuntu']);" >Bug #994882 “php5{en/dis}mod not available in php 5.3.10” : Bugs : “php-apc” package : Ubuntu</a>

日頃CentOSばかり触っているとUbuntuルールにつまづきます。

慣れればUbuntuの整理方法が使いやすいと思うのですが。