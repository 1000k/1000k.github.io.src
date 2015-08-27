---
title: SQLite3で現在時刻をINSERTする
author: 1000k
layout: post
date: 2010-07-05
url: /2010/07/05/insert-current-time-in-sqlite3/
categories:
  - SQL
  - SQLite
tags:
  - SQLite3
  - お役立ち
---
**datetime('now', 'localtime')**を使う。

```sql
INSERT INTO posts (body, created, modified)
VALUES ('ウソ文章', datetime('now', 'localtime'), datetime('now', 'localtime'));
```


なお、この値はスキーマ定義時にDEFAULT値に設定することなどもできるようです。

### 参考

[SQLite3 での日付処理](http://www.tamandua-webtools.net/sqlite3-date.html)