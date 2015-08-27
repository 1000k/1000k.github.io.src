---
title: git の submodule を最新にする方法
author: 1000k
layout: post
date: 2013-10-19
url: /2013/10/19/command-to-update-git-submodule/
categories:
  - その他
  - 開発環境
tags:
  - git
  - TIPS
  - トラブルシューティング
---
「そんなの `git submodule update` で一発だろ」と思っていましたが、全然違いました。このコマンドをいくら叩いても、標準出力には何も表示されず、submodule はちっとも更新されません。

submodule を最新の状態にするコマンドは `git submodule foreach git pull origin master` です。

## 参考

<a href="http://stackoverflow.com/questions/5828324/update-git-submodule" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://stackoverflow.com/questions/5828324/update-git-submodule', 'Update git submodule &#8211; Stack Overflow']);" >Update git submodule &#8211; Stack Overflow</a>