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
**datetime(&#8216;now&#8217;, &#8216;localtime&#8217;)**を使う。

```
INSERT INTO posts (body, created, modified)
VALUES ('ウソ文章', datetime('now', 'localtime'), datetime('now', 'localtime'));
```


なお、この値はスキーマ定義時にDEFAULT値に設定することなどもできるようです。

### 参考

<a href="http://www.tamandua-webtools.net/sqlite3-date.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.tamandua-webtools.net/sqlite3-date.html', 'SQLite3 での日付処理']);" title="SQLite3 での日付処理">SQLite3 での日付処理</a>