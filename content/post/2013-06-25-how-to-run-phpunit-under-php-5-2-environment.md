---
title: 今さら PHP 5.2 環境で PHPUnit を動かせるようにする手順
author: 1000k
layout: post
date: 2013-06-25
url: /2013/06/25/how-to-run-phpunit-under-php-5-2-environment/
categories:
  - PHP
  - PHPUnit
tags:
  - PEAR
  - PHP
  - PHPUnit
  - チュートリアル
---
業務で使っているアプリが PHP 5.2 でしか動かず、仕方なく PHP 5.2 でも動く PHPUnit 3.6.11 を使っています。([= 3.6.12 は PHP 5.3 が必要。']);" title="PHPUnitをアップグレードしたら動作しなくなった | 1000g">PHPUnit >= 3.6.12 は PHP 5.3 が必要。](http://blog.1000k.net/2012/10/19/phpunit%E3%82%92%E3%82%A2%E3%83%83%E3%83%97%E3%82%B0%E3%83%AC%E3%83%BC%E3%83%89%E3%81%97%E3%81%9F%E3%82%89%E5%8B%95%E4%BD%9C%E3%81%97%E3%81%AA%E3%81%8F%E3%81%AA%E3%81%A3%E3%81%9F/))

最近になってサーバーのリプレースの時期になり、新しいビルドマシンに PHPUnit をインストールする羽目になったのですが、うまくバージョンが揃わず苦労しました。

作業メモを残しておきます。

なお、コマンドは全て super user で叩いています。

## 環境

CentOS 6.3 63bit

<!--more-->

## PHP 5.2 をインストールする

そもそも5.2系はとっくに Deprecated なので使うべきではないのですが、業務で使うから仕方ない。

[PHP: Releases](http://www.php.net/releases/) から 5.2.17 をダウンロードして、ソースからインストールします。

```
# wget http://museum.php.net/php5/php-5.2.17.tar.gz
# tar -zxf php-5.2.17.tar.gz
# cd php-5.2.17
# ./configure #{好きな configure オプション}
# make
# make install
```


configure オプションは [PHP: 中心となる configure オプションのリスト - Manual](http://www.php.net/manual/ja/configure.about.php) を参考に。

## PEAR をアップグレードする

PHP 5.2.17 にデフォルトで入っている PEAR 1.9.1 では、各種パッケージをインストールする時に下のようなエラーが出ます。

```
warning: phpunit/PHPUnit requires PEAR Installer (version >= 1.9.4), installed version iphpunit/PHPUnit can optionally use package "phpunit/PHP_Invoker" (version >= 1.1.0)
warning: phpunit/File_Iterator requires PHP (version >= 5.3.3), installed version is 5.2warning: phpunit/File_Iterator requires PEAR Installer (version >= 1.9.4), installed ver1.9.1
warning: phpunit/Text_Template requires PHP (version >= 5.3.3), installed version is 5.2warning: phpunit/Text_Template requires PEAR Installer (version >= 1.9.4), installed ver1.9.1
warning: phpunit/PHP_CodeCoverage requires PHP (version >= 5.3.3), installed version is
warning: phpunit/PHP_CodeCoverage requires PEAR Installer (version >= 1.9.4), installed
is 1.9.1
...
```


まずは PEAR を 1.9.4 以上にアップデートしましょう。

以下のコマンドです。

```
# pear upgrade PEAR
```


## PHPUnit をインストールする

これで PHPUnit をインストールする準備が整いました。

下のコマンドを叩いてください。

```
# pear config-set auto_discover 1
# pear channel-add pear.phpunit.de
# pear install php.phpunit.de/PHPUnit-3.6.11
```


以上で PHPUnit が動作可能になりました。

```
# phpunit -v
PHPUnit 3.6.11 by Sebastian Bergmann.
```


## ついでに関連ツールも入れる

[Template for Jenkins Jobs for PHP Projects](http://jenkins-php.org/) で紹介されているツールは下記のコマンドでインストールできます。

```
# pear channel-add pear.pdepend.org
# pear channel-add pear.phpmd.org
# pear channel-add components.ez.no
# pear channel-add pear.symfony-project.com

# pear install pdepend/PHP_Depend
# pear install phpmd/PHP_PMD
# pear install phpunit/phpcpd
# pear install phpunit/phploc
# pear install PHPDocumentor
# pear install PHP_CodeSniffer
```


## 愚痴

  * [Composer](https://github.com/composer/composer) を使えれば圧倒的に楽なのですが、PHP >= 5.3.2 でしか動かないので諦めました。
  * PEAR 遅い。
  * 自動テストがあれば PHP をバージョンアップできるか検証できるのですが、ほとんどテストケースが無いのでそれも無理。コード総数は10万行。死にたい。

ちゃんと自動テストは作ろうね！