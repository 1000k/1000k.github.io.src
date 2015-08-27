---
title: PHPUnitをアップグレードしたら動作しなくなった
author: 1000k
layout: post
date: 2012-10-19
url: /2012/10/19/phpunit-does-not-run-after-upgrade/
categories:
  - PHP
  - PHPUnit
  - テスト
tags:
  - PEAR
  - PHPUnit
  - トラブルシューティング
---
PHPUnit を何となくアップグレードしたら動かなくなってしまいました。

今回は pear upgrade で PHPUnit を 3.6.11 から 3.6.12 に 更新した後、phpunit コマンドを叩くと下記のようなエラーが出るようになりました。

```
# phpunit

Parse error: syntax error, unexpected T_FUNCTION, expecting ')' in /usr/local/lib/php/PHP/Timer/Autoload.php on line 47

Call Stack:
    0.0018      50328   1. {main}() /usr/local/bin/phpunit:0
    0.0030      87992   2. require('/usr/local/lib/php/PHPUnit/Autoload.php') /usr/local/bin/phpunit:43
```


修復に小一時間かかったので、直し方をメモしておきます。

<!--more-->

## 原因

PHPUnit は PHP >= 5.3 での利用を強く推奨しており、PHP <= 5.2 のままだと動かない機能を盛り込んでくることがあります。

また、本体以外にも多数の依存パッケージがあり、どれかが急に >= 5.3 の機能を使い始めても動かなくなります。

私の環境は PHP 5.2.x だったため、今回のエラーに出くわしました。原因は phpunit/PHP_Timer がバージョンアップしたせいでした。

  * [phpunit/PHP_Timer-1.0.4](https://github.com/sebastianbergmann/php-timer/blob/1.0.4/PHP/Timer/Autoload.php)
  * [phpunit/PHP_Timer-1.0.3](https://github.com/sebastianbergmann/php-timer/blob/1.0.3/PHP/Timer/Autoload.php)

上記のソースからおわかりのように、1.0.4 から匿名関数を使っています。これは PHP >= 5.3 でしか動きません。5.2系ではパースエラーになります。

## 対策

PHP_Timer を再インストールすることで修正可能です。

アンインストールの時は依存関係を無視して削除するため「-n」オプションを付けています。

```
# pear uninstall -n phpunit/PHP_Timer
"phpunit/PHP_Timer" can be optionally used by installed package pear/PHP_CodeSniffer
warning: phpunit/PHP_Timer (version >= 1.0.2) is required by installed package "phpunit/phpcpd"
warning: phpunit/PHP_Timer (version >= 1.0.1, version <= 1.0.3) is required by installed package "phpunit/PHPUnit"
warning: phpunit/PHP_Timer should not be uninstalled, other installed packages depend on this package
uninstall ok: channel://pear.phpunit.de/PHP_Timer-1.0.4

#  pear install phpunit/PHP_Timer-1.0.3
downloading PHP_Timer-1.0.3.tgz ...
Starting to download PHP_Timer-1.0.3.tgz (3,743 bytes)
....done: 3,743 bytes
install ok: channel://pear.phpunit.de/PHP_Timer-1.0.3
```


以上で動作するようになりました。

PHP 5.2系の人はうかつに 「pear upgrade phpunit/phpunit」なんてコマンドを叩かないようにしましょう。

もしくはさっさと5.4系に移行しましょう。

## 参考

  * [How to Downgrade PHPUnit 3.6 to 3.5.15 | Dusty Reagan](http://dustyreagan.com/downgrade-phpunit-3-6-to-3-5-15/)
      * pear パッケージのダウングレード方法はこちらを参考にしました。