---
title: PHPで値がセットされているか確かめる方法
author: 1000k
layout: post
date: 2012-04-18
url: /2012/04/18/how-to-ascertain-whether-the-value-in-php-has-been-set/
categories:
  - PHP
tags:
  - PHP
  - 実験
---
似たようなエントリは世間に無数にありますが、自分用のまとめとして。

PHPで「値がセットされているか」をチェックするには、下記のように何通りか書き方があります。

```
if (isset($var)) {...}
if (!empty($var)) {...}
if ($var) {...}
```


それぞれの違いがあいまいだと痛い目を見るので、違いをハッキリさせておきます。

<!--more-->

## テストコード



## 結果

```
*** NULL ***
isset(NULL): false
!empty(NULL): false
(NULL): false

*** true ***
isset(true): true
!empty(true): true
(true): true

*** false ***
isset(false): true
!empty(false): false
(false): false

*** 'false' ***
isset('false'): true
!empty('false'): true
('false'): true

*** '' ***
isset(''): true
!empty(''): false
(''): false

*** 0 ***
isset(0): true
!empty(0): false
(0): false

*** '0' ***
isset('0'): true
!empty('0'): false
('0'): false

*** 'aaaaa' ***
isset('aaaaa'): true
!empty('aaaaa'): true
('aaaaa'): true

*** 12345 ***
isset(12345): true
!empty(12345): true
(12345): true

*** array () ***
isset(array ()): true
!empty(array ()): false
(array ()): false
```


以上より、「値がセットされているか」が真になるのは、下記の表のようになります。

| -             | isset($var) | !empty($var) | ($var) |
| ------------------- | ----------- | ------------ | ------ |
| NULL                | false       | false        | false  |
| true                | true        | true         | true   |
| false               | true        | false        | false  |
| 'false' | true        | true         | true   |
| "             | true        | false        | false  |
|                     | true        | false        | false  |
| '0'     | true        | false        | false  |
| 'aaaaa' | true        | true         | true   |
| 12345               | true        | true         | true   |
| array()             | true        | false        | false  |

## まとめ

  * issetはNULL以外がセットされていればtrue
  * 「!empty($var)」と「($var)」は同義
  * empty()は大変クセが強いので、[公式マニュアル](http://jp.php.net/manual/ja/function.empty.php)を熟読しましょう。

> varが空でないか、0でない値であれば FALSE を返します。
>
> 次のような値は空であるとみなされます。
>
>   * "" (空文字列)
>   * 0 (整数 の 0)
>   * 0.0 (浮動小数点数の 0)
>   * "0" (文字列 の 0)
>   * NULL
>   * FALSE
>   * array() (空の配列)
>   * var $var; (変数が宣言されているが、クラスの中で値が設定されていない)

## 参考

PHPの公式マニュアルを見て確かめましょう。

  * [PHP: empty - Manual](http://jp.php.net/manual/ja/function.empty.php)
  * [PHP: isset - Manual](http://jp.php.net/manual/ja/function.isset.php)
  * [PHP: if - Manual](http://jp.php.net/manual/ja/control-structures.if.php)