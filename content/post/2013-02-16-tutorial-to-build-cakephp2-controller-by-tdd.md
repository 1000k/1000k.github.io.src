---
title: CakePHP 2 の Controller をテスト駆動で作成するチュートリアル
author: 1000k
layout: post
date: 2013-02-15
url: /2013/02/16/tutorial-to-build-cakephp2-controller-by-tdd/
categories:
  - CakePHP
  - PHP
tags:
  - CakePHP
  - PHP
  - チュートリアル
---
社内でCakePHP 2 を使った TDD を始めたチームがあったので、説明のため書いてみました。

ApisController::index() という、POSTされた値に +1 して返すだけのアクションを TDD で作ります。

せっかく CakePHP なので、bake コマンドを使って進めます。

<!--more-->

## 1. ダミーのテーブルを作成する

bake コマンドはテーブルに紐付いていない MVC を作れません。

以下、Apis テーブルがあるものとして話を進めます。

XAMPP を使っているなら、下記コマンドで Mojamoja データベースに Apis テーブルを作成できます。

```
$ c:\xampp\mysql\bin\mysql -uroot Mojamoja
mysql> CREATE TABLE `apis` (
  `id` int(11) NOT NULL,
  `created` date,
  `modified` date,
  `name` text
) ENGINE=InnoDB;
```


## 2. 必要なファイルをbakeする

```
$ cd c:/mojamoja/cake2/app
$ ./Console/cake.bat bake controller apis
$ ./Console/cake.bat bake view apis```


テストしたいのは Controller だけですが、対応する View が無いとテスト実行時にエラーになるので一緒に作っておきます。

ちなみに逆順でコマンドを叩くとエラーが出ます。View を作る時には、対応する Controller が必要になるためです。

この時点でテストを動かすことが可能です。

```
$ Console\cake.bat test app controller/ApisController

Welcome to CakePHP v2.2.5 Console
---------------------------------------------------------------
App : app
Path: c:\work\htdocs\cake2_tdd_tutorial\app\
---------------------------------------------------------------
CakePHP Test Shell
---------------------------------------------------------------
PHPUnit 3.7.10 by Sebastian Bergmann.

[41;37mF[0m

Time: 2 seconds, Memory: 4.25Mb

There was 1 failure:

1) Warning
No tests found in class "ApisControllerTest".

C:\work\htdocs\cake2_tdd_tutorial\lib\Cake\TestSuite\CakeTestRunner.php:59
C:\work\htdocs\cake2_tdd_tutorial\lib\Cake\TestSuite\CakeTestSuiteCommand.php:113
C:\work\htdocs\cake2_tdd_tutorial\lib\Cake\Console\Command\TestShell.php:274
C:\work\htdocs\cake2_tdd_tutorial\lib\Cake\Console\Command\TestShell.php:259
C:\work\htdocs\cake2_tdd_tutorial\lib\Cake\Console\Shell.php:395
C:\work\htdocs\cake2_tdd_tutorial\lib\Cake\Console\ShellDispatcher.php:201
C:\work\htdocs\cake2_tdd_tutorial\lib\Cake\Console\ShellDispatcher.php:69

[37;41m[2KFAILURES!
[0m[37;41m[2KTests: 1, Assertions: 0, Failures: 1.
[0m[2K
```


「テストケースが1個もねえよ」と怒られて終了します。

当然の結果なので次に進みましょう。

## 3. テストケースを作る

ApisController::index() で実現したい仕様に沿って、テストケースを書きます。

**app\Test\Case\Controller\ApisControllerTest.php** は下記のようになります。

```
<?php
App::uses('ApisController', 'Controller');

class ApisControllerTest extends ControllerTestCase {

    public $fixtures = array();    // どの Fixture も参照しないようにする

    /**
     * @covers ApiController::index
     */
    public function testIndex() {
        // 期待される値
        $expected = array('uso' => 801);

        // ApisController::index() を叩いた動作を再現する。
        $this->testAction(
            '/apis/index',
            array(
                'data' => array('uso' => 800),  // uso=800 を POST する
                'return' => 'vars'              // set された値を $this->vars に格納する
            )
        );

        // $this->vars には、$this->testAction() 実行時に
        // Controller から View に渡った値が格納される。
        $this->assertEquals($expected, $this->vars);
    }

}
```


$this->testAction($action, $options) は、Controller を叩いた時の挙動をシミュレートするメソッドです。今回は uso=800 という値を POST している状況を再現しています。

第2引数の $options には様々なオプションを指定できます。「'return' => 'vars'」を指定することで、testAction() 実行時に Controller から View に渡った値を、$this->vars に格納することができます。

この時点でテストを叩くと、当然まだ ApisController::index() が実装できていないので、エラーが出て終了します。

このようにエラーになる状態を **Red** と呼びます。

この後実際のコードを書いてテストを成功する状態、すなわち **Green** にするのがテスト駆動開発 (TDD: Test Driven Development) です。

## 4. とにかく成功するコードを書く

**この作業の前に View/Apis/index.ctp の中身を全て空にしておいてください。**

そうしないと、なにやら単体テスト時に余計な文字列が紛れ込んで失敗してしまいます。

**app/Controller/ApisController.php** を以下のように実装します。

```
<?php
App::uses('AppController', 'Controller');

class ApisController extends AppController {

    // どの Model も使わないようにする。
    public $uses = false;

    public function index() {
        $data = array('uso' => 801);
        $this->set($data);
    }

}
```


これでテストを再び実行します。今度は Green になります。

```
$ Console\cake.bat test app controller/ApisController


Welcome to CakePHP v2.2.5 Console
---------------------------------------------------------------
App : app
Path: c:\work\htdocs\cake2_tdd_tutorial\app\
---------------------------------------------------------------
CakePHP Test Shell
---------------------------------------------------------------
PHPUnit 3.7.10 by Sebastian Bergmann.

.

Time: 0 seconds, Memory: 6.00Mb

[30;42m[2KOK (1 test, 1 assertion)
[0m[2K
```


テストケース内で予測している値をそのまんま返しているので、成功するのは当たり前です。

もしこの時点で失敗した場合は、どこか別の部分で間違っている可能性があります。

この後はコードの実装とテストを繰り返し、最終的に Green にします。

## 5. コードを実装する

最終的に実装したコードは下記のようになります。

**app/Controller/ApisController.php**

```
<?php
App::uses('AppController', 'Controller');

class ApisController extends AppController {

    // どの Model も使わないようにする。
    public $uses = false;

    public function index() {
        $data = array();

        foreach ($_POST as $key => $value) {
            $data[$key] = $value + 1;
        }

        $this->set($data);
    }
}
```


実装コードを Green にする際には、「とにかく早く書くこと」が大切です。

最初から美しさを求めてコーディングするのは辞めたほうがいいでしょう。

このコードはテストケースによって保護されているので、後からいくらでも洗練できます。

このように後からコードを洗練することを **リファクタリング** と呼びます。

## まとめ

TDD は以下のような流れで、Red->Green をリズミカルに行います。

  1. テストケースを書く。
  2. テストを走らせ、失敗することを確認する。(Red)
  3. 必ずテストが成功する実装コードを書く。(Green)
  4. 実装とテストを繰り返しながら、最終的に Green になる実装コードを書く。

ゆとり期間にコードをリファクタリングしましょう。

そうすることでコードのメンテナンス性を保つことができます。

## 補足

[公式チュートリアル](http://book.cakephp.org/2.0/en/development/testing.html#testing-controllers) はそこそこ学べますが、なぜか結果を debug() でコンソールに表示してるだけで、アサーション（期待される値と実際の結果の照合）をやってません。これはダメです。

戻り値はちゃんと assert しましょう。モックを使って挙動を確かめるなら expectation を使いましょう。

あと、現在の実装だと $_POST が空の時に Warning が出るので、実際にはもう少しエンハンスが必要になるでしょう。

## 参考になるサイト

  * [CakePHP API](http://api21.cakephp.org)
      * CakeBook は CakePHP の機能の概要を知るのに向いていますが、メソッドの使い方を調べるだけならこっちの方が早いです。
  * [PHPUnit 公式マニュアル](http://www.phpunit.de/manual/3.6/ja/index.html)
      * CakePHP2 の CakeTestCase クラスは、PHPUnit の簡単なラッパーなので、全ての機能が使えます。こちらのドキュメントも読んでおくといいです。
  * [PHPUnit のアサーション一覧](http://www.phpunit.de/manual/3.6/ja/writing-tests-for-phpunit.html#writing-tests-for-phpunit.assertions)
      * ほとんどの場合は assertEquals() でカバーできますが、知っておくと便利なものも多いです。