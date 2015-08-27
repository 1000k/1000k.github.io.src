---
title: CakePHP1.3.2でSQLite3を使う（改訂版）
author: 1000k
layout: post
date: 2010-06-30
url: /2010/06/30/using-sqlite3-in-cakephp-132/
categories:
  - CakePHP
  - PHP
tags:
  - CakePHP
  - SQLite
---
bakeできなかったので、下記を参考にやり直しました。

[CakePHP SQLite - Stack Overflow](http://stackoverflow.com/questions/1021980/cakephp-sqlite/2241385#2241385)

bake all するとWarningが続々出ますが、一応動きました。

  1. [cakephp's datasources at master - GitHub](http://github.com/cakephp/datasources) から「CakePHP Datasources Plugin v0.2」をダウンロードする
  2. 「cakePHPディレクトリ/app/plugins/datasources」内に解凍する
  3. プラグイン内の「models/datasources/dbo/dbo_sqlite3.php」を編集し、「class DboSqlite3 extends DboSource {」より前の行に「App::import('Datasource','DboSource');」を追加する

```php
<?php
/**
 * SQLite layer for DBO.
 *
 * PHP versions 4 and 5
 *
 * CakePHP(tm) : Rapid Development Framework (http://cakephp.org)
 * Copyright 2005-2009, Cake Software Foundation, Inc. (http://cakefoundation.org)
 *
 * Licensed under The MIT License
 * Redistributions of files must retain the above copyright notice.
 *
 * @copyright     Copyright 2005-2009, Cake Software Foundation, Inc. (http://cakefoundation.org)
 * @link          http://cakephp.org CakePHP(tm) Project
 * @package       datasources
 * @subpackage    datasources.models.datasources.dbo
 * @since         CakePHP Datasources v 0.1
 * @license       MIT License (http://www.opensource.org/licenses/mit-license.php)
 */

App::import('Datasource','DboSource');

/**
 * DBO implementation for the SQLite3 DBMS.
 *
 * A DboSource adapter for SQLite 3 using PDO
 *
 * @package datasources
 * @subpackage datasources.models.datasources.dbo
 */
class DboSqlite3 extends DboSource {
```

2\. 「/app/config/database.php」を以下のように設定する


```php
    var $default = array(
        'driver' => 'Datasources.DboSqlite3',
        'database' => '/full/path/to/db/db.sqlite',
    );
```



  これで何とかbakeできるようになる。ただし、何かにつけ「`Warning: FIXME: Can't parse field:  in C:\xampp\htdocs\cake\sandbox\cake\libs\model\datasources\dbo_source.php on line 2510`」みたいなエラーが出てくる。


とりあえず[CakePHP1.3の公式チュートリアル](http://book.cakephp.org/view/1543/Simple-Acl-controlled-Application)をやってみたけど、一応普通に動きました。
