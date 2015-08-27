---
title: CakePHP公式のブログ作成チュートリアルをSQLite3でやってみる
author: 1000k
layout: post
date: 2010-07-06
url: /2010/07/06/cakephp-official-tutorial-using-sqlite3/
categories:
  - CakePHP
  - PHP
  - SQLite
tags:
  - CakePHP
  - SQLite
  - チュートリアル
---
### このチュートリアルの目的

一つの CakePHP コアに複数のアプリケーションを同居させる方法を学べます。DB を作って database.php を作成すれば、あとはbakeするだけで基本的な画面が出来上がります。

### 公式チュートリアルと今回のやり方の違い

[公式チュートリアル](http://book.cakephp.org/view/1527/Tutorials-Examples)

  * MySQL でなく SQLite3 で作る
  * アプリを CakePHP 解凍時の app ディレクトリ内に作らず、「{インストールディレクトリ}/blog」内に bake したファイルを元に作る
      * 一つの CakePHP コアから複数のアプリを併存させる環境を想定
  * Model/View/Controller は bake して作る

<!--more-->

### 作成環境

  * CakePHP 1.3.2
  * XAMPP Windows

    | 用途                  | 場所                        |
    | ------------------- | ------------------------- |
    | ドキュメントルート           | c:\xampp\htdocs\          |
    | CakePHPインストールディレクトリ | c:\xampp\htdocs\cake      |
    | blogアプリの場所          | c:\xampp\htdocs\cake\blog |

### 手順

#### 1. SQLite3を使うための dbo_sqlite3.php を導入する

導入先: **{インストールディレクトリ}/cake/libs/model/datasources/dbo/dbo_sqlite3.php**

#### 2. コンソールからblogアプリをbakeする

```
cd c:\xampp\htdocs\cake
mkdir blog
cake/console/cake bake blog
```


対話式コマンドは↓の通り（Databese Configurationは後で直接いじるので適当でよい）

```
Welcome to CakePHP v1.3.2 Console
---------------------------------------------------------------
App : sandbox
Path: c:\xampp\htdocs\cake
---------------------------------------------------------------
Bake Project
Skel Directory: C:\xampp\htdocs\cake\cake\console\templates\skel
Will be copied to: c:\xampp\htdocs\cake\blog
---------------------------------------------------------------
Look okay? (y/n/q)
[y] > y
Do you want verbose output? (y/n)
[n] >
---------------------------------------------------------------
Created: blog in c:\xampp\htdocs\cake\blog
---------------------------------------------------------------

Creating file c:\xampp\htdocs\cake\blog\views\pages\home.ctp
Wrote `c:\xampp\htdocs\cake\blog\views\pages\home.ctp`
Welcome page created
Random hash key created for 'Security.salt'
Random seed created for 'Security.cipherSeed'
CAKE_CORE_INCLUDE_PATH set to C:\xampp\htdocs\cake in webroot/index.php
CAKE_CORE_INCLUDE_PATH set to C:\xampp\htdocs\cake in webroot/test.php
Remember to check these value after moving to production server
Your database configuration was not found. Take a moment to create one.
---------------------------------------------------------------
Database Configuration:
---------------------------------------------------------------
Name:
[default] >
Driver: (db2/firebird/mssql/mysql/mysqli/odbc/oracle/postgres/sqlite/sybase)
[mysql] > sqlite3
Driver: (db2/firebird/mssql/mysql/mysqli/odbc/oracle/postgres/sqlite/sybase)
[mysql] > sqlite
Persistent Connection? (y/n)
[n] >
Database Host:
[localhost] >
Port?
[n] >
User:
[root] >
Password:
>
The password you supplied was empty. Use an empty password? (y/n)
[n] > y
Database Name:
[cake] >
Table Prefix?
[n] >
Table encoding?
[n] >

---------------------------------------------------------------
The following database configuration will be created:
---------------------------------------------------------------
Name:         default
Driver:       sqlite
Persistent:   false
Host:         localhost
User:         root
Pass:
Database:     cake
---------------------------------------------------------------
Look okay? (y/n)
[y] >
Do you wish to add another database configuration?
[n] >

Creating file c:\xampp\htdocs\cake\blog\config\database.php
Wrote `c:\xampp\htdocs\cake\blog\config\database.php`
```


#### 3. DBファイルを作成し、テーブルとテストデータを作成する

DBの作成先: **{インストールディレクトリ}/blog/db/blog.sqlite3**

DBファイルを開いて、下記SQLを実行する。

```
-- テーブルを作成する
CREATE TABLE posts (
    id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    title TEXT,
    body TEXT,
    created DATETIME DEFAULT NULL,
    modified DATETIME DEFAULT NULL
);

-- テストデータを流し込む
INSERT INTO posts (title,body,created) VALUES ('The title', 'This is the post body.', datetime("now", "localtime"));
INSERT INTO posts (title,body,created) VALUES ('A title once again', 'And the post body follows.', datetime("now", "localtime"));
INSERT INTO posts (title,body,created) VALUES ('Title strikes back', 'This is really exciting! Not.', datetime("now", "localtime"));
```


#### 4. database.phpを編集する

編集するファイル: **{インストールディレクトリ}/blog/config/database.php**

```
<?php
class DATABASE_CONFIG {

    var $default = array(
        'driver' => 'sqlite3',
        'database' => 'C:/xampp/htdocs/cake/blog/db/blog.sqlite3',
    );
}
?>
```


ここまでやったら、http://localhost/cake/blog にアクセスして、「Sweet, "Blog" got Baked by CakePHP!」という画面が出ることを確認します。画面に赤か黄色で示されるエラーがあれば、どこか間違っているので直してください。

※画像やCSSが表示されない場合、.htaccessの設定が違う場合があります。下記を手がかりに解決してください。

  * [mod_rewriteについて :: CakePHPブログチュートリアル :: 開発例 :: マニュアル :: 1.3コレクション :: The Cookbook](http://book.cakephp.org/ja/view/1533/A-Note-on-mod_rewrite)
  * [Apacheとmod_rewrite :: インストール :: CakePHPによる開発 :: マニュアル :: 1.2コレクション :: The Cookbook](http://book.cakephp.org/ja/view/37/Apache-and-mod_rewrite-and-htaccess)
  * [CakePHP mod_rewriteについて - happy-luckyのPHP5日記](http://d.hatena.ne.jp/happy-lucky/20080208/p1)
  * [技術/Apache/mod_rewriteメモ(1):RewriteBaseの誤解 - Glamenv-Septzen.net](http://www.glamenv-septzen.net/view/167)
  * [動的ページを静的ページのように見せる方法 - ItsMemo::IT (旧)](http://www.itsmemo.com/it/web/000173.html)

#### 5. Model/View/Controllerをbakeする

チュートリアルだと「/cake/console/にPATHを設定しろ」と書いていますが、面倒なのでやってません。

コンソールから、下記のように入力してbakeします。

```
cd c:/xampp/htdocs/cake/blog
../cake/console/cake bake
```


この後、何をするか対話式ダイアログで聞かれるので、「M」→「C」→「V」の順に作成してください。（Controllerより先にViewを作ろうとすると「まだControllerがないから作れません」と言われます）

### 6. 完成

http://localhost/cake/blog/posts にアクセスすると、画面ができあがってます。