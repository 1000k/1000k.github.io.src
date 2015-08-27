---
title: MySQL 5.1 の boolean 型を検証
author: 1000k
layout: post
date: 2010-11-15
url: /2010/11/15/anatomy-of-mysql-boolean-type/
categories:
  - MySQL
  - SQL
tags:
  - MySQL
  - 型
---
MySQL にて BOOLEAN 型のフィールドを作りたいと思ったのですが、Googleで調べると「MySQL の BOOLEAN 型は実質 tinyint(1) と同じ」といったことが書いてありました。

また、「MySQL で厳密な BOOLEAN 型を表現したければ、ENUM('TRUE', 'FALSE') を使った方がいい」とも書いてありました。

個人的には SQL 文に「WHERE flag IS TRUE」と書いて SELECT できれば文句ありません。そういった挙動ができるのか確認しました。

実験した環境の MySQL バージョンは 5.1.41 です。

<!--more-->

## データはどのように格納されるのか？

以下のように、連番とブール値を対に持つテーブルを作りました。

```
CREATE TABLE boolean_test (
    `id` INT NOT NULL AUTO_INCREMENT,
    `flag` BOOLEAN,
    PRIMARY KEY (`id`)
);
```


テストデータは下記の通りです。ブーリアン（TRUE/FALSE）のほか、整数値や NULL でもどうなるのか実験します。

```
INSERT INTO boolean_test VALUES (1, TRUE);
INSERT INTO boolean_test VALUES (2, FALSE);
INSERT INTO boolean_test VALUES (3, 1);
INSERT INTO boolean_test VALUES (4, 0);
INSERT INTO boolean_test VALUES (5, NULL);
INSERT INTO boolean_test VALUES (6, 127);
INSERT INTO boolean_test VALUES (7, 128);
INSERT INTO boolean_test VALUES (8, 2147483647); -- 4 byteの最大値
INSERT INTO boolean_test VALUES (9, 2147483648); -- 4 byteの最大値+1
```


作成結果は下のようになりました。

```
mysql> SELECT * FROM boolean_test;
+----+------+
| id | flag |
+----+------+
|  1 |    1 |
|  2 |    0 |
|  3 |    1 |
|  4 |    0 |
|  5 | NULL |
|  6 |  127 |
|  7 |  127 |
|  8 |  127 |
|  9 |  127 |
+----+------+
9 rows in set (0.00 sec)
```


この時点で、TRUE は 1 として、FALSE は 0 として格納されています。

また、どんなに大きな値を入れても 127 で止まっています。

確かに MySQL 内部では tinyint(1) として管理されているのがわかります。

実際、テーブルを DESCRIBE すると、型は tinyint(1) と表示されます。

```
mysql> DESCRIBE boolean_test;
+-------+------------+------+-----+---------+----------------+
| Field | Type       | Null | Key | Default | Extra          |
+-------+------------+------+-----+---------+----------------+
| id    | int(11)    | NO   | PRI | NULL    | auto_increment |
| flag  | tinyint(1) | YES  |     | NULL    |                |
+-------+------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)
```


## BOOLEAN型としてSELECTできるか？

### 検索条件を「=」で指定してみる

```
SELECT * FROM boolean_test WHERE flag = TRUE;  -- id:1, 3 がヒット
SELECT * FROM boolean_test WHERE flag = FALSE; -- id:2, 4 がヒット
SELECT * FROM boolean_test WHERE flag = 1;     -- id:1, 3 がヒット
SELECT * FROM boolean_test WHERE flag = 0;     -- id:2, 4 がヒット
```


「TRUE = 1」「FALSE = 0」と扱っているようです。その証拠に、値が127のレコードは「flag = TRUE」でもヒットしません。

→**「flag = TRUE」という条件は、「flag = 1」と同値。**

### 検索条件を「IS」で指定してみる

```
SELECT * FROM boolean_test WHERE flag IS TRUE;  -- id:1, 3, 6, 7, 8, 9 がヒット
SELECT * FROM boolean_test WHERE flag IS FALSE; -- id:2, 4 がヒット
```


0 より大きい値は TRUE として、0 は FALSE として判別される模様。

→**「flag = TRUE」と「flag IS TRUE」は全く別の結果を返す。**

## UNKNOWN はどういう扱いになる？

SQL-99 の定義によると、「BOOLEAN 型は TRUE/FALSE/UNKNOWN の3値で表現される」ようです。

ref: [新しい業界標準「SQL99」詳細解説](http://www.atmarkit.co.jp/fnetwork/tokusyuu/01sql99/sql99_2a.html)

> SQL99では本来の意味であるTRUE（真）とFALSE（偽）およびUNKNOWN（不明）を値として持つ真理値型が提供される。

SQL 標準の BOOLEAN カラムには NULL は無いんですね。

MySQL ではどういう風に呼び出せるのか確かめてみました。

```
SELECT * FROM boolean_test WHERE flag IS UNKNOWN;  -- id: 5 がヒット
SELECT * FROM boolean_test WHERE flag = UNKNOWN;   -- NG: #1054 - Unknown column 'UNKNOWN' in 'where clause'
SELECT * FROM boolean_test WHERE flag = 'UNKNOWN'; -- ???: id:2, 4 がヒット
```


NULL のフィールドが UNKNOWN として表現されているようです。

また、2行目のように「= UNKNOWN」と指定するとエラーが出ました。「IS UNKNOWN」で指定しなければならないようです。

よくわからないのが3行目。id:2, 4はFALSEのレコードなので、全くヒットしてほしくないのですが。

このあたりは MySQL の仕様をよく確認しないとわかりませんが、まず入力しないだろうということで、この際無視します。

## 考察

やはり MySQL の BOOLEAN 型は tinyint(1) と同じようです。

Boolean 型と思ってクエリを投げると意図しない結果が返ってきてしまう危険性があるので、注意が必要です。

また、ENUM 型は検証していませんが、SQL標準（SQL99）ではサポートされていないため、プログラムの互換性を考えると積極的に使う気が起きません。

## まとめ

MySQL で Boolean 型を表したければ、少し気持ち悪いですが tinyint(1) でカラムを作った方が良いようです。

## 参考

  * [コメント: mySQLには、boolean型が無いなんて・・ - スラッシュドット・ジャパン](http://slashdot.jp/comments.pl?threshold=0&mode=thread&commentsort=1&sid=446159)
  * [新しい業界標準「SQL99」詳細解説](http://www.atmarkit.co.jp/fnetwork/tokusyuu/01sql99/sql99_2a.html)
  * [Ｃ言語編　第１０章　変数のサイズ](http://www.geocities.jp/ky_webid/c/010.html)
  * [追補: SQLデータ型の一覧 | Netsphere Laboratories](http://www.nslabs.jp/book2-sql-types.rhtml)