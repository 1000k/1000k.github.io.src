---
title: vfsStream の使い方
author: 1000k
layout: post
date: 2013-03-27
url: /2013/03/27/usage-of-vfsstream/
categories:
  - PHP
  - PHPUnit
  - テスト
tags:
  - PHPUnit
  - チュートリアル
  - テスト
---
PHPUnit でファイルシステムのテストを行うとき便利な <a href="http://vfs.bovigo.org/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://vfs.bovigo.org/', 'vfsStream']);" >vfsStream</a> ですが、簡単なサンプルがあまり無かったので書いてみました。

<!--more-->

## vfsStream とは？

  * ファイルの読み書きをテストする時に使うフレームワーク。
  * 仮想ファイルシステムを作成し、その中でディレクトリやファイルを操作できる。
      * 実ファイルシステム: _file://&#8230;_
      * 仮想ファイルシステム: _vfs://&#8230;_
  * 実ファイルを作成せずにテストできるので、テストファイルが散らばる可能性が無く、前回のテストのゴミを意識せずに済む。
  * ディレクトリやファイルの権限/ユーザー/オーナーも再現可能。

## インストール方法

詳しくは以前書いた <a href="http://blog.1000k.net/2012/09/04/vfsstream%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/2012/09/04/vfsstream%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B/', 'vfsStreamをインストールする | 1000g']);" >vfsStreamをインストールする | 1000g</a> を参照。

### PHP < 5.3 の場合

```
$ pear channel-discover pear.bovigo.org
$ pear install bovigo/vfsStream-beta
```


### PHP >= 5.3 の場合

composer.json に以下のように書く。

```
"mikey179/vfsStream": "v1.1.0"
```


利用可能なバージョンは <a href="https://packagist.org/packages/mikey179/vfsStream" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://packagist.org/packages/mikey179/vfsStream', 'mikey179/vfsStream &#8211; Packagist']);" >mikey179/vfsStream &#8211; Packagist</a> を参照。

## サンプルコード

単純にファイルにテキストを追記するだけのロガーをテストしてみます。

なお、vfsStream 0.12 を利用しています。

_Logger.php_

```
Class Logger {
  public static function log($str, $path) {
    return file_put_contents($path, $str);
  }
}
```


_LoggerTest.php_

```
require_once 'Logger.php';
require_once 'vfsStream/vfsStream.php';

class LoggerTest extends PHPUnit_Framework_Test {
    /**
     * @var vfsStreamDirectory
     */
    private $root;

    protected function setUp() {
        // 仮想ファイルシステムにルートディレクトリを作る。
        $this->root = vfsStream::setup();     // "vfs://root" ディレクトリが作成される

        // ファイルのパスは vfsStream::url() で取得する。
        var_dump(vfsStream::url('root'));         // => vfs://root
        var_dump(is_dir(vfsStream::url('root'))); // => true
    }

    /**
     * @covers Logger::log
     */
    public function testLog() {
        $str = 'Lorem ipsum';
        $path = vfsStream::url('root/foo.txt');

        $this->assertGreaterThan(0, Logger::log($str, $path));
        $this->assertEquals($str, file_get_contents($path));
    }
}
```


## 参考

  * <a href="http://tech.vg.no/2011/03/09/mocking-the-file-system-using-phpunit-and-vfsstream/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://tech.vg.no/2011/03/09/mocking-the-file-system-using-phpunit-and-vfsstream/', 'Mocking the file system using PHPUnit and vfsStream – VG Tech']);" >Mocking the file system using PHPUnit and vfsStream – VG Tech</a>
      * vfsStream を使った様々なテストのサンプルがあります。
  * <a href="https://github.com/mikey179/vfsStream/wiki" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://github.com/mikey179/vfsStream/wiki', 'Home · mikey179/vfsStream Wiki']);" >Home · mikey179/vfsStream Wiki</a>
      * 作成元の GitHub ページ。
  * <a href="http://www.phpunit.de/manual/current/ja/test-doubles.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.phpunit.de/manual/current/ja/test-doubles.html', '第10章 テストダブル']);" >第10章 テストダブル</a>
      * PHPUnit のドキュメント内にある解説。