---
title: Markdown記法で連番リストを無視したい
author: 1000k
layout: post
date: 2010-07-05
url: /2010/07/05/escape-ordered-list-in-markdown-syntax/
categories:
  - その他
tags:
  - Markdown
---
Markdown記法では、行頭に「1.」「2.」などと書くと、勝手に連番リストにされてしまいます。

それが嫌な場合は、「**999&#92;.**」というように、「&#92;.」でエスケープしてやるとリストになりません。ただのパラグラフになります。

### 参考

[Daring Fireball: Markdown Syntax Documentation](http://daringfireball.net/projects/markdown/syntax)