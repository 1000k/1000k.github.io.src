---
title: SQLiteのインメモリDBを異なるPDOオブジェクトから参照するには
author: 1000k
layout: post
date: 2011-11-11
url: /2011/11/11/how-to-refer-sqlite-in-memory-db-from-defferent-pdo-object/
categories:
  - PHP
  - SQLite
tags:
  - PDO
  - PHP
  - SQLite
---
どうにもSQLiteのインメモリDBの値が取得できなかったので、実験してみました。

いきなり結論を書くと、**「あるPDOオブジェクトからは、別のPDOオブジェクトが作成したインメモリDBには接続できない」**ようです。

# テストコード

```
<?php
/**
 * SQLiteのオンメモリDBにテストデータを作成し、PDOからアクセスできることを確かめる
 */
try {
    $pdo1 = new PDO('sqlite::memory:', null, null);
    $pdo1->exec('CREATE TABLE uso(id INTEGER PRIMARY KEY, name STRING)');
    $pdo1->exec('INSERT INTO uso VALUES (1, "uhouho");');
    $pdo1->exec('INSERT INTO uso VALUES (2, "mojamoja");');

    // 接続済みのPDOオブジェクトからデータを取得できるか試してみる
    $stmt = $pdo1->query('SELECT * FROM uso');
    if (!$stmt) {
        echo 'Cannot fetch data via $pdo1.';
    } else {
        print_r($stmt->fetchAll(PDO::FETCH_ASSOC));
    }

    // 新たにPDOオブジェクトを作成し、データを取得できるか試してみる
    $pdo2 = new PDO('sqlite::memory:');
    $stmt = $pdo2->query('SELECT * FROM uso');
    if (!$stmt) {
        echo 'Cannot fetch data via $pdo2.';
    } else {
        print_r($stmt->fetchAll(PDO::FETCH_ASSOC));
    }
} catch (Exception $e) {
    exit($e->getMessage());
}
```


# 結果

```
Array
(
    [0] => Array
        (
            [id] => 1
            [name] => uhouho
        )

    [1] => Array
        (
            [id] => 2
            [name] => mojamoja
        )

)
Cannot fetch data via $pdo2.
```


$pdo1で作成したインメモリDBに、$pdo2からは接続できていません。

どちらも同じ「sqlite::memory:」ですが、見ている領域は異なるようです。

# 考察

インメモリDB内に作ったテーブルに、複数のPDOオブジェクトからアクセスすることはできませんでした。

アクセスしたければ、テーブルを作ったPDOオブジェクトを使い回すしかなさそうです。