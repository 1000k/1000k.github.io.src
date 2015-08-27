---
title: CakePHP コントローラからビューに渡る変数をアサーションする
author: 1000k
layout: post
date: 2010-07-20
url: /2010/07/20/assert-vars-from-controller-to-view/
categories:
  - CakePHP
  - PHP
  - SimpleTest
tags:
  - CakePHP
  - PHP
  - UnitTest
---
公式マニュアルの「<a href="http://book.cakephp.org/ja/view/366/Testing-controllers" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://book.cakephp.org/ja/view/366/Testing-controllers', 'コントローラのテスト']);" title="コントローラのテスト :: テスト(Testing) :: CakePHPによる作業の定石 :: マニュアル :: 1.2コレクション :: The Cookbook">コントローラのテスト</a>」では、ただ単に変数をdebugして吐いてるだけで一切アサーションをしていません。これってテストと言えるのでしょうか？

コントローラのメソッドが実行された後、仕様通りに値が入っているかチェックするにはどうすればいいでしょうか？

<!--more-->

コントローラ内でsetされた変数は **viewVars** というメンバ変数に格納されるようです。例えば、下記のようなコントローラがあった場合。

```
// app/controllers/usos_controller.php
class UsosController extends AppController {
    var $name = 'Usos';

    function hige() {
        $this->set('moja', $this->Uso->find("all"));
    }
}
?>
```


これをコントローラのテストクラスで取得するには下記のようにすれば良いです。

```
// app/tests/cases/controllers/usos_controller.test.php
<?php
/* Usos Test cases generated on: 2010-07-16 16:07:53 : 1279264793*/
App::import('Controller', 'Usos');

class TestUsosController extends UsosController {
    var $autoRender = false;

    function redirect($url, $status = null, $exit = true) {
        $this->redirectUrl = $url;
    }

    function render($action = null, $layout = null, $file = null) {
        $this->renderedAction = $action;
    }

    function _stop($status = 0) {
        $this->stopped = $status;
    }
}


class UsosControllerTestCase extends CakeTestCase {
    var $fixtures = array('app.uso');

    function startTest() {
        $this->Usos =&#038; new TestUsosController();
        $this->Usos->constructClasses();
    }

    function endTest() {
        unset($this->Usos);
        ClassRegistry::flush();
    }

    function testHige() {
        $this->Usos->hige();
        $result = $this->Usos->viewVars;     // Usos::viewVars に、set された値が格納されている
        var_dump($result);
    }
}
?>
```


出力結果

```
array(1) {
  ["moja"]=>
  array(1) {
    [0]=>
    array(1) {
      ["Uso"]=>
      array(4) {
        ["id"]=>
        string(1) "1"
        ["name"]=>
        string(11) "Miles Davis"
        ["created"]=>
        string(19) "2010-07-16 15:00:49"
        ["modified"]=>
        string(19) "2010-07-16 15:00:49"
      }
    }
  }
}
```


### 参考

  * <a href="http://mark-story.com/posts/view/anatomy-of-a-cakephp-test-case" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://mark-story.com/posts/view/anatomy-of-a-cakephp-test-case', 'Anatomy of a CakePHP Test Case | Mark Story']);" title="Anatomy of a CakePHP Test Case | Mark Story">Anatomy of a CakePHP Test Case | Mark Story</a>
  * <a href="http://mark-story.com/posts/view/testing-cakephp-controllers-the-hard-way" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://mark-story.com/posts/view/testing-cakephp-controllers-the-hard-way', 'Testing CakePHP Controllers the hard way | Mark Story']);" title="Testing CakePHP Controllers the hard way | Mark Story">Testing CakePHP Controllers the hard way | Mark Story</a>
  * <a href="http://cakephp.1045679.n5.nabble.com/Getting-viewVars-set-in-a-controller-when-using-Mock-objects-during-testing-td1318031.html#a1318031" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://cakephp.1045679.n5.nabble.com/Getting-viewVars-set-in-a-controller-when-using-Mock-objects-during-testing-td1318031.html#a1318031', 'Nabble &#8211; CakePHP &#8211; Getting viewVars set in a controller when using Mock objects during testing']);" title="Nabble - CakePHP - Getting viewVars set in a controller when using Mock objects during testing">Nabble &#8211; CakePHP &#8211; Getting viewVars set in a controller when using Mock objects during testing</a>