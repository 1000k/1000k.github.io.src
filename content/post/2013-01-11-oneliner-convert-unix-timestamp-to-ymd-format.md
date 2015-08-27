---
title: UnixタイムスタンプをYMD形式に変換するワンライナー
author: 1000k
layout: post
date: 2013-01-11
url: /2013/01/11/oneliner-convert-unix-timestamp-to-ymd-format/
categories:
  - Linux
  - PHP
  - Ruby
  - コマンド
tags:
  - PHP
  - Ruby
  - ワンライナー
---
Unixタイムスタンプを読みやすい形式に変更するワンライナーを、いくつかの言語で紹介します。

## シェル

```
$ date -d @1357872483
2013年  1月 11日 金曜日 11:48:03 JST
```


## PHP

```
$ php -r 'echo date("r", 1357872483);'
Fri, 11 Jan 2013 11:48:03 +090
```


date()関数の第1引数でフォーマットを変更できます。

詳しくは [PHP: date - Manual](http://php.net/manual/ja/function.date.php) を参照。

## Ruby

```
$ ruby -e 'p Time.at(1357872483)'
Fri Jan 11 11:48:03 +0900 2013
```


## 参考

  * [How to convert timestamps to dates in bash? - Stack Overflow](http://stackoverflow.com/questions/2371248/how-to-convert-timestamps-to-dates-in-bash)
  * [UNIX timestampを通常時刻に変換したい時 - OpenGroove](http://open-groove.net/linux/change-unix-timestamp/)