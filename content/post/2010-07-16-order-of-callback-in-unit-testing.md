---
title: CakePHP 単体テストで使うメソッドのコールバックの実行順序
author: 1000k
layout: post
date: 2010-07-16
url: /2010/07/16/order-of-callback-in-unit-testing/
categories:
  - CakePHP
  - PHP
  - SimpleTest
tags:
  - CakePHP
  - SimpleTest
  - UnitTest
---
公式マニュアルだけではSimpleTestのコールバックの実行される順序がわからなかったので、調べた結果をメモしておきます。

<!--more-->

下のようなコードを実行してみました。実際のテストメソッドは「testHige()」です。

```
<?php
App::import('Controller', 'Usos');

class TestUsosController extends UsosController {
    var $autoRender = false;

    function redirect($url, $status = null, $exit = true) {
        $this->redirectUrl = $url;
    }
}

class UsosControllerTestCase extends CakeTestCase {
    var $fixtures = array('app.uso');

    function startTest() {
        echo("startTest\n");
        $this->Usos =& new TestUsosController();
        $this->Usos->constructClasses();
    }

    function endTest() {
        echo("endTest\n");
        unset($this->Usos);
        ClassRegistry::flush();
    }

    function start() {
        echo("start\n");
    }

    function end() {
        echo("end\n");
    }

    function startCase() {
        echo("startCase\n");
    }

    function endCase() {
        echo("endCase\n");
    }

    function before($method) {
        echo($method . " before\n");
    }

    function after($method) {
        echo($method . " after\n");
    }

    // 実際のテストメソッド
    function testHige() {
        echo("testHige\n");
    }
}
?>
```


実行結果は以下の通りです。

```
start before
start
start after
startCase before
startCase
startCase after
testHige before
testHige
testHige after
endCase before
endCase
endCase after
end before
end
end after
```


大まかに書くと「start() → startCase() → testHige() → endCase() → end()」の順に実施され、それぞれのメソッドの前後にbefore($method) と after($method) が呼び出されるようです。

### 参考

  * [CakeTestCase Callback Methods :: テストの作成 :: テスト(Testing) :: CakePHPによる作業の定石 :: マニュアル :: 1.3コレクション :: The Cookbook](http://book.cakephp.org/ja/view/1206/CakeTestCase-Callback-Methods)