---
title: CakePHP モデルを使わないコンポーネントの単体テスト手順
author: 1000k
layout: post
date: 2010-07-21
url: /2010/07/21/how-to-test-component/
categories:
  - CakePHP
  - PHP
  - SimpleTest
---
CakePHP 1.3.2でコンポーネントを単体テストする場合、モデルの有無によって大幅に面倒くささが変わってきます。なぜならコンポーネントはモデルを直接操作できず、モデルを操作するためのコントローラを経由する必要があるからです。

公式マニュアルにある方法はモデルを使うケースのみで、しかもわかりづらいです (どのファイルのどのブロックに書いてるのかわからない)。今回はモデル無しのコンポーネントのテスト方法をメモしておきます。

<!--more-->

### やり方

以下のコンポーネント、「HigeComponent」をテストする場合を考えます。

```
// app/controllers/components/hige.php
<?php
class HigeComponent extends Object {
    function moja() {
        return "mojamoja";
    }
}
?>
```


テストケースは下記の通り、コンポーネントを読み込んで直接実行するだけです。

```
// app/tests/cases/components/hige.test.php
<?php
App::import("Component", "Hige");

class HigeComponentTestCase extends CakeTestCase {
    function setUp() {
        $this->component = new HigeComponent();
    }

    // テストケース
    function testMoja() {
        $result = $this->component->moja();
        $expected = "moja";

        $this->assertEqual($result, $expected);
    }
}
?>
```


あとは、下記の通りコマンドを打ち込めばOK。（cake.batにパスを通さずやってます）

```
cd c:\xampp\htdocs\cakephp\app
../cake/console/cake testsuite app case components/hige
```


出力結果

```
Welcome to CakePHP v1.3.2 Console
---------------------------------------------------------------
App : app
Path: c:\xampp\htdocs\cakephp\app
---------------------------------------------------------------
CakePHP Test Shell
---------------------------------------------------------------
Running app case components/hige
1/1 test cases complete: 1 passes.
Time taken by tests (in seconds): 0.016170024871826
Peak memory use: (in bytes): 11,534,288
```


### 参考

  * <a href="http://book.cakephp.org/ja/view/1216/Testing-components" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://book.cakephp.org/ja/view/1216/Testing-components', 'コンポーネントのテスト :: テスト(Testing) :: CakePHPによる作業の定石 :: マニュアル :: 1.3コレクション :: The Cookbook']);" title="コンポーネントのテスト :: テスト(Testing) :: CakePHPによる作業の定石 :: マニュアル :: 1.3コレクション :: The Cookbook">コンポーネントのテスト :: テスト(Testing) :: CakePHPによる作業の定石 :: マニュアル :: 1.3コレクション :: The Cookbook</a>
  * <a href="http://book.cakephp.org/ja/view/995/Configuring-Components" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://book.cakephp.org/ja/view/995/Configuring-Components', 'コンポーネントの設定 :: コンポーネント :: CakePHPによる開発 :: マニュアル :: 1.3コレクション :: The Cookbook']);" title="コンポーネントの設定 :: コンポーネント :: CakePHPによる開発 :: マニュアル :: 1.3コレクション :: The Cookbook">コンポーネントの設定 :: コンポーネント :: CakePHPによる開発 :: マニュアル :: 1.3コレクション :: The Cookbook</a>
  * <a href="http://www.littlehart.net/atthekeyboard/2007/06/26/testing-components-in-cakephp/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.littlehart.net/atthekeyboard/2007/06/26/testing-components-in-cakephp/', '@TheKeyboard » Blog Archive » Testing Components In CakePHP']);" title="@TheKeyboard » Blog Archive » Testing Components In CakePHP">@TheKeyboard » Blog Archive » Testing Components In CakePHP</a>

assertionは下記を参考に。

  * <a href="http://www.syuhari.jp/blog/archives/438" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.syuhari.jp/blog/archives/438', 'CakePHP1.2 SimpleTest 値を検証する assert～メソッド | Sun Limited Mt.']);" title="CakePHP1.2 SimpleTest 値を検証する assert～メソッド | Sun Limited Mt.">CakePHP1.2 SimpleTest 値を検証する assert～メソッド | Sun Limited Mt.</a>
  * <a href="http://simpletest.org/api/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://simpletest.org/api/', 'SimpleTest 1.1beta Documentation']);" title="SimpleTest 1.1beta Documentation">SimpleTest 1.1beta Documentation</a>
  * <a href="http://www.debuggable.com/posts/unit-testing-in-cakephp-part-1---introduction-to-unit-testing:48102610-c5d0-4398-a010-76974834cda3" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.debuggable.com/posts/unit-testing-in-cakephp-part-1---introduction-to-unit-testing:48102610-c5d0-4398-a010-76974834cda3', 'Unit Testing in CakePHP Part 1 &#8211; Introduction to Unit Testing » Debuggable Ltd']);" title="Unit Testing in CakePHP Part 1 - Introduction to Unit Testing » Debuggable Ltd">Unit Testing in CakePHP Part 1 &#8211; Introduction to Unit Testing » Debuggable Ltd</a>
  * <a href="http://simpletest.sourceforge.net/en/unit_test_documentation.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://simpletest.sourceforge.net/en/unit_test_documentation.html', 'PHP Unit Test documentation']);" title="PHP Unit Test documentation">PHP Unit Test documentation</a>