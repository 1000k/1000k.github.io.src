---
title: MariaDB 10.0.1 で日本語が文字化けしないようにする
author: 1000k
layout: post
date: 2013-04-14
url: /2013/04/14/avoid-garbled-japanese-in-mariadb-10-0-1/
categories:
  - MariaDB
tags:
  - MariaDB
  - トラブルシューティング
---
MariaDB 10.0.1 で確認しました。

MySQL 同様、MariaDB はデフォルトの文字セットが latin1 になっているので、日本語をそのまま格納して表示しようとすると文字化けします。

日本語を利用可能にするためには、`/etc/my.cnf.d/server.cnf` に以下の行を追加します。

```
[mysqld]
character-set-server = utf8
collation-server     = utf8_general_ci
skip-character-set-client-handshake
```


これで MariaDB の再起動後、日本語が正しく出力されるようになっています。

## 参考

  * [(・∀・) ozamasa:MacのMySQLを5.5から5.6にバージョンアップしたときのメモ。](http://ozamasa.naganoblog.jp/e1194854.html)