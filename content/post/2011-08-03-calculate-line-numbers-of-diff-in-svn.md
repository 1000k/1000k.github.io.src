---
title: svnでdiffの行数を取得する
author: 1000k
layout: post
date: 2011-08-03
url: /2011/08/03/calculate-line-numbers-of-diff-in-svn/
categories:
  - Linux
  - コマンド
  - その他
  - 開発環境
tags:
  - Linux
  - Subversion
  - svn
  - TIPS
  - コマンド
---
どれだけコーディングしたかチェックするため、svnのdiffの行数を取得するコマンドを考えました。

下のコマンドを叩くと、変更された行の総数が表示されます。

**(2011-08-17修正) 以前のコードはdiffのヘッダ行まで取得してしまっていたので、正しい数値が得られるよう修正しました。**

```
svn diff -r {before}[:{after}] -x -b {path} | grep -E '^[+\-][[:blank:]]' | wc -l
```


例えば「チェンジセット301～310で uso.php に発生した差分の総行数」は、下のようになります。

```
# svn diff -r 301:310 -x -b ./uso.php | grep -E '^[+\-][[:blank:]]' | wc -l
500
```


ロジックは、

  1. 空白文字の差分（インデントや空行の削除）を無視してdiffを取る
  2. 差分行だけ(「+ 」「- 」どちらかから始まっている行)を取得する
  3. 行数をカウントする

となっています。

# 参考

  * [svn diffで-wオプションを使う — ディノオープンラボラトリ](http://openlab.dino.co.jp/2007/10/23/184825129.html)
  * [Linux である文字を含む行を削除したい - Enjoi Blog](http://blog.enjoitech.jp/article/71)