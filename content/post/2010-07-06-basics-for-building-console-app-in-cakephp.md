---
title: CakePHPでコンソールアプリを作るときの基本
author: 1000k
layout: post
date: 2010-07-06
url: /2010/07/06/basics-for-building-console-app-in-cakephp/
categories:
  - CakePHP
  - PHP
tags:
  - CakePHP
  - PHP
  - コンソール
---
コンソールアプリケーションの保存先: /app/vendors/shell

基本的な構造

```php
// /app/vendors/shell/uso.php
<?php
class UsoShell extends Shell {
    // モデルを使う場合、配列$usesで指定する
    var $uses = array("Post");

    function main() {
        // ここに命令を記述する
        // ...

        // 標準出力 (自動改行する)
        $this->out('hige');

        // 標準出力 (自動改行しない)
        $this->out('moja', false);

        // 標準エラー出力
        $this->err('bosa');

        // 区切り線
        $this->hr();

        // 異常終了させる
        $this->error("異常発見", "異常終了します");
    }
}
?>
```


実行方法

```
cd /app
../cake/console/cake uso
```


出力結果

```
$ ../cake/console/cake uso


Welcome to CakePHP v1.3.2 Console
---------------------------------------------------------------
App : uso
Path: c:\xampp\htdocs\cake\app
---------------------------------------------------------------
hige
mojabosa
---------------------------------------------------------------
Error: 異常発見
異常終了します
```


### 参考

  * [CakePHPコンソールのシェルを作るときのメモ - ぷぎがぽぎ](http://d.hatena.ne.jp/brtRiver/20090427/1240802118)
  * [シェルやタスクを作成する :: CakePHP コンソール :: CakePHPによる開発 :: マニュアル :: 1.2コレクション :: The Cookbook](http://book.cakephp.org/ja/view/110/Creating-Shells-Tasks)