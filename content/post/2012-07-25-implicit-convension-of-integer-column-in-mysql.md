---
title: MySQLのINTEGER型カラムに0を入れると予期しないWHERE句にヒットする
author: 1000k
layout: post
date: 2012-07-25
url: /2012/07/25/implicit-convension-of-integer-column-in-mysql/
categories:
  - MySQL
  - SQL
tags:
  - MySQL
  - トラブルシューティング
---
**「MySQLの暗黙の型変換には気をつけろ！」**というお話です。

<!--more-->

下記のように、INTEGER型のカラムと、TEXT型のカラムを持つ簡単なテーブルがある場合を考えます。

```
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| id    | int(11) | YES  |     | NULL    |       |
| name  | text    | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
```


入力したレコードは下記の通り。

```
mysql> INSERT INTO users VALUES(0, "name0");
mysql> INSERT INTO users VALUES(0, "name1");

+------+-------+
| id   | name  |
+------+-------+
|    0 | name0 |
|    1 | name1 |
+------+-------+
```


ここで、下記のようなクエリを投げてみます。

```
mysql> SELECT * FROM users WHERE id = 'mojamoja';
```


普通に考えれば、何もヒットするはずはありません。

ところが結果は下記のようになります。

```
+------+-------+
| id   | name  |
+------+-------+
|    0 | name0 |
+------+-------+

1 row in set, 1 warning (0.00 sec)
```


なぜか「id = 0」のカラムにヒットします。

絶対ヒットしてほしくないのですが。

よく見ると、「1 warning」と書いてあります。

SHOW WARNINGSコマンドで確かめてみましょう。

```
mysql> SHOW WARNINGS;
+---------+------+--------------------------------------------+
| Level   | Code | Message                                    |
+---------+------+--------------------------------------------+
| Warning | 1292 | Truncated incorrect DOUBLE value: 'uhouho' |
+---------+------+--------------------------------------------+
1 row in set (0.00 sec)
```


「不正なDOUBLE型の値'uhouho'を切り捨てました」と書いてあります。どうやら、MySQLの_暗黙の型変換_が悪さをしているようです。

[MySQL :: MySQL 5.1 リファレンスマニュアル :: 11.1.2 式評価でのタイプ変換](http://dev.mysql.com/doc/refman/5.1/ja/type-conversion.html) に淡々と箇条書きされていることを引用します。

>   * 比較の演算の両方の引数がストリングの場合、それらはストリングとして比較されます。
>   * 両方の引数が整数の場合、それらは整数として比較されます。
>   * … 中略 …
>   * 他のすべてのケースでは、引数は浮動少数点 ( 実 ) 数として比較されます。

```
mysql> SELECT 1 > '6x';
        -> 0
mysql> SELECT 7 > '6x';
        -> 1
mysql> SELECT 0 > 'x6';
        -> 0
mysql> SELECT 0 = 'x6';
        -> 1                -- 文字列「'x6'」が実数「0」に変換されてしまっている
```


この被害に遭いたくなければ、まずは**カラムの型と違う型をWHERE句に指定しない**ようにしましょう。

そのほかの対策としては、MySQLのSQLモードを設定することで、型チェックを厳格にすることもできるようです（未検証）。

## 参考

  * [MySQLの文字列型から数値型への自動型変換が意味不明すぎる - hnwの日記](http://d.hatena.ne.jp/hnw/20120405)
      * 文字列が浮動小数点に切り替えられる問題を詳しく解説。
  * [undefined - 徳丸浩の日記(2009-09-24)](http://www.tokumaru.org/d/20090924.html#p01)
      * SQLインジェクション対策時にはまりやすいことの説明。値を文字列にキャストしているときに遭遇しうる不具合について解説。
  * [MySQL :: MySQL 5.1 リファレンスマニュアル :: 4.2.6 SQL モード](http://dev.mysql.com/doc/refman/5.1/ja/server-sql-mode.html)
      * 値チェックのモード。不正な日付や0除算などのチェックを厳格にできる。
  * [MySQL :: MySQL 5.1 リファレンスマニュアル :: 11.1.2 式評価でのタイプ変換](http://dev.mysql.com/doc/refman/5.1/ja/type-conversion.html)