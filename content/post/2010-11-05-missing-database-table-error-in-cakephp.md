---
title: CakePHPでいわれのない「Missing database table」エラーを直すには
author: 1000k
layout: post
date: 2010-11-05
url: /2010/11/05/missing-database-table-error-in-cakephp/
categories:
  - CakePHP
  - PHP
tags:
  - bake
  - CakePHP
  - トラブルシューティング
---
モデルの名前を修正して、再度Bakeしようとすると以下のようなエラーが。

```
Error: Missing database table 'people' for model 'Person'
```


「people」テーブルはさっきDBから消したので、存在していません。いったい何を見てエラーを出しているのか。

Googleでエラーメッセージを検索すると、「**/app/tmp/cache/**配下のキャッシュを消せば直る」という記事が。さっそく消して見るも、やはり同じエラーが出ました。

思い切って **/app/models**配下にあるモデルをすべて消したところ、無事bakeができるようになりました。

作成済みのモデルも見てリレーションを判別していたのでしょうか？何という想定外な仕様。