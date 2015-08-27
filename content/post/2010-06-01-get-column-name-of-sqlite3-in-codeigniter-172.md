---
title: CodeIgniter1.7.2でSQLite3のカラム名を取得できるようにする
author: 1000k
layout: post
date: 2010-06-01
url: /2010/06/01/get-column-name-of-sqlite3-in-codeigniter-172/
categories:
  - CodeIgniter
  - PHP
tags:
  - CodeIgniter
  - SQLite
---
CodeIgniterのSQLite3に対する迫害と、引き続き奮闘しています。

CIでSQLite3を使えるようにする方法は <a href="http://blog.1000k.net/2010/05/26/codeigniter-codeigniter1-7-2-%E3%81%A7-sqlite3-%E3%81%AEdb%E3%82%92%E4%BD%BF%E3%81%86/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/2010/05/26/codeigniter-codeigniter1-7-2-%E3%81%A7-sqlite3-%E3%81%AEdb%E3%82%92%E4%BD%BF%E3%81%86/', '先日']);" title="CodeIgniter1.7.2 で SQLite3 のDBを使う">先日</a> 紹介しました。

CIにはテーブルのカラム名を取得する「$this->db->list\_fields()」というメソッドがありますが、上の方法で導入したPDOドライバでは使えません。どうやら list\_fields() メソッドから呼び出されるローカル関数が定義されていないようです。

ちょっと改造すれば使えるようになったので、やり方を共有します。

### 手順

  1. /system/database/drivers/pdo/pdo_driver.php 内に、下記メソッドを追加する。（このメソッドはMySQLドライバ内の同一メソッドをSQLite用に修正したものです）

```
/**
 * Show column query
 *
 * Generates a platform-specific query string so that the column names can be fetched
 *
 * @access  public
 * @param   string  the table name
 * @return  string
 */
function _list_columns($table = '')
{
    return "PRAGMA table_info(" . $table . ");";
}
```


  1. /system/database/DB_driver.php 内の l.833-835 を下記のように書き換えます。

```
if (isset($row['COLUMN_NAME']))
{
    $retval[] = $row['COLUMN_NAME'];
}

 ↓

if (isset($row['COLUMN_NAME']) || isset($row['name']))
{
    $retval[] = isset($row['COLUMN_NAME']) ? $row['COLUMN_NAME'] : $row['name'];
}
```


これで、SQLite3でも 「$this->db->list_fields()」 を使ってカラム名が取得できるようになります。