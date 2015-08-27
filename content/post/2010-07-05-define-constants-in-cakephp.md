---
title: CakePHP で独自定数を定義する方法
author: 1000k
layout: post
date: 2010-07-05
url: /2010/07/05/define-constants-in-cakephp/
categories:
  - CakePHP
  - PHP
tags:
  - CakePHP
---
defineで定義するパターンと、連想配列で定義するパターンがあります。基本的に後者の方が便利です。

<!--more-->

### defineで定義する方法

1. **/app/config/bootstrap.php** に下記を追加する

```php
// /app/config/bootstrap.php
<?php
config(‘const’);
?>
```


2. **/app/config/const.php** に定義したい定数を追加する

```php
// /app/config/const.php
define(’USO’,'800’);
```


これで全ファイルから定数USOを呼び出せるようになっています。

### 連想配列で定義する方法

1. app/config/ 配下に、任意のコンフィグファイルを作成します。ここでは **hige.php** とします。

```php
// app/config/hige.php
<?php
$config["Band1"] = array(
    "name"  => "Thelonious Monk Quartet",
    "piano" => "Thelonious Monk",
    "sax"   => "Charlie Rouse",
    "bass"  => "Larry Gales",
    "drums" => "Ben Riley",
);

$config["Band2"] = array(
    "name"      => "Miles Davis Quintet",
    "trumpet"   => "Miles Davis",
    "sax"       => "John Coltrane",
    "piano"     => "Wynton Kelly",
    "bass"      => "Paul Chambers",
    "drums"     => "Jimmy Cobb",
);

?>
```


2. **app/config/bootstrap.php** に下の行を追加します。

```php
Configure::load("hige");
```


3. これで任意のスクリプトから設定した値を読み込めるようになりました。例えばコンソールに出力する場合、下記のようになります。

```
// app/vendors/shells/moja.php
<?php
    // エントリーポイント
    function main() {
        $this->out(Configure::read("Band1"));
        $this->out(Configure::read("Band1.name"));

        $this->hr();

        $this->out(Configure::read("Band2"));
        $this->out(Configure::read("Band2.name"));
    }
}
?>
```


出力結果

```
Thelonious Monk Quartet
Thelonious Monk
Charlie Rouse
Larry Gales
Ben Riley
Thelonious Monk Quartet
---------------------------------------------------------------
Miles Davis Quintet
Miles Davis
John Coltrane
Wynton Kelly
Paul Chambers
Jimmy Cobb
Miles Davis Quintet
```


### 参考

  * [アプリケーション独自の設定情報(Configureクラス) - PHP学習日記](http://php.sunvisor.net/2008/01/configure.html)
  * [【CakePHP】独自の定数の書き方 | ねねとまつの小部屋](http://blog.ne2ma2.com/archives/154)
  * [ライブラリクラス :: 1.2から1.3への移行ガイド :: 付録 :: マニュアル :: 1.3コレクション :: The Cookbook](http://book.cakephp.org/ja/view/1565/Library-Classes)