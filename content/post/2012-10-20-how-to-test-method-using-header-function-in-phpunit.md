---
title: header()関数を使ったメソッドをPHPUnitでテストする方法
author: 1000k
layout: post
date: 2012-10-20
url: /2012/10/20/how-to-test-method-using-header-function-in-phpunit/
categories:
  - PHP
  - PHPUnit
  - テスト
tags:
  - PHPUnit
  - テスト
  - トラブルシューティング
---
PHPUnit はテスト中のメッセージを逐一標準出力するため、header() 関数を用いたメソッドのテストができません。

header() 関数はその実行前の何らかの標準出力がされていると「Cannot modify header information」というエラーを吐いてしまいます。

以下、テストコードと対策をメモします。

<!--more-->

## 現象

たとえば下記のようなテストケースはエラーで止まります。

```
class Api {
    public function output($data, $response_code = 200) {
        http_response_code($response_code);
        header('Content-Type: application/json'); // PHPUnitの標準出力のせいで、テストケース中にエラーになる
        $output = json_encode($data);

        echo $output;
        return $output;
    }
}

class ApiTest extends \PHPUnit_Framework_TestCase {
    // テストケース
    /**
     * @covers Api::output
     * @dataProvider forTestOutput
     */
    public function testOutput($data, $response_code) {
        ob_start();
        $actual = $this->object->output($data, $response_code);
        $output = ob_get_contents();
        $expected = json_encode($data);

        $this->assertEquals($expected, $actual);
        $this->assertEquals($output, $actual);
        $this->assertEquals(http_response_code(), $response_code);
        ob_clean();
    }
}
```


以下のようなエラーを吐きます。

```
Cannot modify header information - headers already sent by (output started at D:\xampp\php\pear\PHPUnit\Util\Printer.php:172)
```


## 対策

**@runInSeparateProcess** アノテーションを利用することで、テストケースだけを別プロセスで実行することができます。

つまり、標準出力が真っさらな状態でテストを開始できるので、header() 関数がエラーを吐きません。

```
/**
     * @covers Api::output
     * @dataProvider forTestOutput
     * @runInSeparateProcess
     */
    public function testOutput($data, $response_code) {
        // ...
    }
```


```
PHPUnit 3.7.8 by Sebastian Bergmann.

.

Time: 3 seconds, Memory: 2.75Mb

OK (1 test, 3 assertions)
```


header() 関数を使ったメソッドのテストをする際には覚えておきたいテクニックです。

## 参考

  * [付録B アノテーション](http://www.phpunit.de/manual/3.7/ja/appendixes.annotations.html#appendixes.annotations.runInSeparateProcess)
  * [PHP: header - Manual](http://www.php.net/manual/ja/function.header.php)
  * [unit testing - Test PHP headers with PHPunit - Stack Overflow](http://stackoverflow.com/questions/9745080/test-php-headers-with-phpunit)