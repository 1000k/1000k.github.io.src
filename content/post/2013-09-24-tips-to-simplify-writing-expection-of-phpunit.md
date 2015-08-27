---
title: PHPUnit の Expectation を簡潔に書く TIPS
author: 1000k
layout: post
date: 2013-09-24
url: /2013/09/24/tips-to-simplify-writing-expection-of-phpunit/
categories:
  - PHP
  - PHPUnit
  - テスト
tags:
  - PHP
  - PHPUnit
  - TIPS
  - テスト
---
PHPUnit の小ネタです。

PHPUnit でモックを使ったテストケースを書くと、以下のように記述量が多くなってうんざりします。

```
<?php
class FooClassTest extends PHPUnit_Framework_TestCase {
    /**
     * @covers FooClass::doSomething
     */
    public function testDoSomething() {
        // FooClass が依存している BarClass のモックを作る。
        // 何もしないように、__construct() と __clone() は無効化しておく。
        $mockBar = $this->getMockBuilder('BarClass')
            ->disableOriginalConstructor()
            ->disableOriginalClone()
            ->getMock();

        // BarClass::barMethod のモックの挙動を変える。
        $mockBar->expect($this->any())
            ->method('barMethod')
            ->will($this->returnValue(true));

        $object = new FooClass($mockBar);

        $this->assertTrue($object->doSomething());
    }
}
```


長い、書きづらい、読みづらい。こんなテストケース、2つ以上書きたくないですね。もっと楽に書くための TIPS をメモしておきます。

<!--more-->

まず、何もしない「プレーンな」モックオブジェクトを作るメソッドを作りましょう。`PHPUnit_Framework_TestCase` を継承した `MyTestCase` に実装してやります。

```
class MyTestCase extends PHPUnit_Framework_TestCase {

    /**
     * 元のクラスが完全に何もしないよう擬装したモックを作成する。
     *
     * - コンストラクタが無効
     * - クローンコンストラクタが無効
     *
     * @param string $class_name
     * @return mixed 指定したクラスのモック
     * @throws InvalidArgumentException 指定したクラス名が見つからない場合
     */
    protected function _getPlainMock($class_name) {
        if (!class_exists($class_name)) {
            throw new InvalidArgumentException("{$class_name} does not exist.");
        }

        return $this->getMockBuilder($class_name)
            ->disableOriginalConstructor()
            ->disableOriginalClone()
            ->getMock();
    }

}
```


また、PHPUnit の Expectation はメソッドチェーンで連続して指定できます ("->" 演算子で繋げて書ける) が、そもそも記述量が多いです。いちいち this this 書くのも煩わしい。

そこで、Expectation をセットするためのメソッドを作ってやると楽になります。

```
<?php
    /**
     * モックオブジェクトに Expectation を設定する。
     *
     * @param Object &$mock モックオブジェクト
     * @param string $matcher マッチャー ([any|once|never|...])
     * @param string $method メソッド名
     * @param mixed $return_value 期待する戻り値
     */
    protected function _setExpectation(&$mock, $matcher, $method, $return_value = null) {
        $mock->expects($this->$matcher())
            ->method($method)
            ->will($this->returnValue($return_value));
    }
```


これを使うと、一番最初のテストケースは以下のように書き直せます。

```
require_once 'MyTestCase.php';

class FooClassTest extends MyTestCase {
    /**
     * @covers FooClass::doSomething
     * @test
     */
    function doSomething() {
        $mockBar = $this->_getPlainMock('BarClass')
        $this->_setExpectation($mockBar, 'any', 'barMethod', true);

        $object = new FooClass($mockBar);

        $this->assertTrue($object->doSomething());
    }
}
```


簡単になりましたね！9行もあったコードが4行に減りました。

ちなみに、さり気なくテストケースの `public` は削除しています。PHP の仕様上、public なメソッドはわざわざそれを書く必要がありません。

また、PHPDoc 部分に `@test` アノテーションを付けると、メソッド名を `test` で始めなくてもテストケースとして認識されるようになります。私はこの方が読みやすいので、最近はこう書くことが多いです。

こういう改善をしても、Ruby に慣れているとまだ長く感じます。「$」「'」「>」などの記号を打つのも煩わしいです。Ruby 使いてえなあ。

## 参考

  * [ 第10章 テストダブル']);" >PHPUnit 公式マニュアル > 第10章 テストダブル](http://phpunit.de/manual/3.8/ja/test-doubles.html)
  * [付録B アノテーション](http://phpunit.de/manual/3.8/ja/appendixes.annotations.html)