---
title: CakePHP で 2個以上のフィールドを結合して displayField に表示するには
author: 1000k
layout: post
date: 2010-11-08
url: /2010/11/08/display-joined-fields-in-cakephp/
categories:
  - CakePHP
  - PHP
tags:
  - CakePHP
  - TIPS
---
displayField に2個以上のフィールドを結合して出したいことがあります。例えば、「姓」「名」フィールドを繋げるケース。

そんなときは、モデル内で **$virtualFields** を指定してやれば良いです。

## 使い方

例えば、$displayFieldに「姓(family\_name)」と「名(given\_name)」を繋げて表示したい場合は、下のようにします。

```
<?php
class User extends AppModel {
    var $name = 'User';
    var $virtualFields = array('full_name' => 'CONCAT(User.family_name , " ", User.given_name)');
    var $displayField = 'full_name';
    var $order = 'User.id ASC';
?>
```


## 注意

  * $virtualFields は **CakePHP 1.3** から導入されたらしいので、それ以前のバージョンでは使えません。
  * 紛らわしいですが、「virtualField**s**」(複数形)です。「displayField」（単数形）と混同しないように。
  * 上記はMySQLでの例です（CONCAT関数で文字列連結している）。PostgreSQLなどでは違うコマンドになるので、各RDBMSのマニュアルを参照してください。

## 参考

  * [virtualFields使ってみた | Web関係のメモ書き](http://yohrk.hp2.jp/archives/18)
  * [バーチャルフィールド :: モデル :: CakePHPによる開発 :: マニュアル :: 1.3コレクション :: The Cookbook](http://book.cakephp.org/ja/view/1608/Virtual-fields)