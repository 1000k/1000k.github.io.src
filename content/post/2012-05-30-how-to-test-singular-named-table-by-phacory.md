---
title: Phactoryで単数形のテーブルをテストする
author: 1000k
layout: post
date: 2012-05-30
url: /2012/05/30/how-to-test-singular-named-table-by-phacory/
categories:
  - PHP
  - PHPUnit
  - テスト
tags:
  - Phactory
  - UnitTest
  - トラブルシューティング
---
<a href="http://phactory.org/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://phactory.org/', 'Phactory']);" title="Phactory - A Database Factory for PHP Unit Tests">Phactory</a>はRoRライクな設計思想のため、テーブル名を勝手に複数形に変換（pluralize）します。

このせいで単数形のテーブル名がテストできない場合、下記のようにdefine()の前に**setInflector()**を使えば直ります。

```
// dtb_shopというテーブルをテストしたい
    protected function setUp() {
        $pdo = new PDO(DB_TYPE . ':dbname=' . DB_NAME . ';host=' . DB_SERVER . ';port=' . DB_PORT, DB_USER, DB_PASSWORD);
        $pdo->exec('CREATE TABLE dtb_shop (shop_id INTEGER, shop_key TEXT)');

        Phactory::setConnection($pdo);
        Phactory::reset();
        Phactory::setInflection('dtb_shop', 'dtb_shop');    // 勝手に「dtb_shops」にされるのを防ぐ
        Phactory::define('dtb_shop', array('shop_id' => '$n', 'shop_key' => 'test shop $n'));

        $this->object = new ShopModel;
    }
```


参考: <a href="https://github.com/chriskite/phactory/issues/3" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://github.com/chriskite/phactory/issues/3', 'Issue #3: Can&#8217;t create singular tables · chriskite/phactory · GitHub']);" title="Issue #3: Can't create singular tables · chriskite/phactory · GitHub">Issue #3: Can&#8217;t create singular tables · chriskite/phactory · GitHub</a>