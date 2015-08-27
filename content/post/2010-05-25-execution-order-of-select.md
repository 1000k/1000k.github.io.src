---
title: '[SQL]SELECT文の処理の順番'
author: 1000k
layout: post
date: 2010-05-25
url: /2010/05/25/execution-order-of-select/
categories:
  - SQL
tags:
  - SQL
---
SELECT文をよく使うわりに、内部でどういう順で処理されているのか知らなかったので調べました。メモしておきます。

  1. FROM
  2. WHERE
  3. GROUP BY
  4. HAVING
  5. SELECT
  6. UNION等の集合演算
  7. ORDER BY
  8. DISTINCT

### 参考

  * <a href="http://java-etc.cocolog-nifty.com/blog/2007/02/post_a311.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://java-etc.cocolog-nifty.com/blog/2007/02/post_a311.html', 'ＳＱＬの魔術: Javaの日々']);" title="ＳＱＬの魔術: Javaの日々">ＳＱＬの魔術: Javaの日々</a>
  * <a href="http://www.postgresql.jp/document/pg814doc/html/sql-select.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.postgresql.jp/document/pg814doc/html/sql-select.html', 'SELECT']);" title="SELECT">SELECT</a>