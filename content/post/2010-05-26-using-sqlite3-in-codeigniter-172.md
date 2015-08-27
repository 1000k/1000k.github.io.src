---
title: '[CodeIgniter] CodeIgniter1.7.2 で SQLite3 のDBを使う'
author: 1000k
layout: post
date: 2010-05-26
url: /2010/05/26/using-sqlite3-in-codeigniter-172/
categories:
  - CodeIgniter
  - PHP
tags:
  - CodeIgniter
  - SQLite
  - トラブルシューティング
---
CodeIgniterはPHP4との下位互換性を保っているせいか、標準で入っているSQLiteドライバーはSQLite2用のものらしい。PDOでのアクセス時は、その古いSQLiteドライバーに基づいているらしく、まともに動かない。

そういうわけで、有志が作成したPDOドライバーを導入しないとSQLite3のDBにはアクセスできない。

以下に、ドライバーの導入手順を示します。

### 手順

※CodeIgniter 1.7.2 用です。それより前のバージョンだと導入するドライバが違うかもしれないので、[Wiki | CodeIgniter](http://codeigniter.com/wiki/PDO_SQLite3/)あたりを読んでください。

  1. [Wiki | CodeIgniter](http://codeigniter.com/wiki/PDO_SQLite3/) から、「pdo driver 0 02 by xi.zip」をダウンロード
  2. CodeIgniter/system/database/drivers/pdo というディレクトリを作成し、その中に解凍する
  3. CodeIgniter/system/application/config/database.php を下記のように編集する

```php
// DBファイルが /system/appilication/db/hige.sqlite3 にある場合
$db['default']['hostname'] = "";
$db['default']['username'] = "";
$db['default']['password'] = "";
$db['default']['database'] = "sqlite:".APPPATH."db/hige.sqlite3";
$db['default']['dbdriver'] = "pdo";
$db['default']['dbprefix'] = "";
```


これでアクセスできるようになります。

ちゃんとロードできているか確かめたい場合は、適当なviewファイルに

```php
<?php echo $this->db->platform() . " ". $this->db->version(); ?>
```


と書いてみて、「pdo 3.6.20」などと出力されればOK。

### 参考

  * [Wiki | CodeIgniter](http://codeigniter.com/wiki/PDO_SQLite3/)
  * [CodeIgniter & SQLite3](http://blog.trevorbramble.com/past/2009/9/20/codeigniter_sqlite3/)