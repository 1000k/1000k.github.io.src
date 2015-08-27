---
title: vmstatの値をロギングする
author: 1000k
layout: post
date: 2011-09-28
url: /2011/09/28/logging-vmstat-values/
categories:
  - Linux
  - コマンド
tags:
  - Linux
  - TIPS
  - vmstat
  - サーバー
  - 監視
  - 運用
---
vmstatの値をファイルに出力したいとき、次のコマンドが使えます。

```
nohup vmstat -n -S M 1 | awk '{ print strftime("%Y/%m/%d %H:%M:%S"), $0 } { system(":") }' >> /path/to/log &
```


出力は下記のようになります。

```
2011/09/28 15:51:07 procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu------
2011/09/28 15:51:07  r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
2011/09/28 15:51:07  1  0      0    251    153    358    0    0     0     9    1    0  0  0 99  0  0
2011/09/28 15:51:08  0  0      0    251    153    358    0    0     0     0 1012   40  0  1 99  0  0
2011/09/28 15:51:09  0  0      0    251    153    358    0    0     0     0 1004   37  0  0 100  0  0
```


このプロセスを終了したい場合は、下記のようにkillしてやればOKです。

```
$ pgrep vmstat      # vmstatのprocess idを取得する
29248

$ kill {上で出てきたpid}
```


# 参考

  * [サーバのリソースを定期的に出力しとく方法：なんとなしの日記](http://babyp.blog55.fc2.com/blog-entry-446.html)
  * [（今さら） vmstat の結果に時間をつけてファイルに出力する - あしのあしあと](http://d.hatena.ne.jp/higher_tomorrow/20110601/1306887919)
  * [【 nohup 】 ログアウトした後もコマンドを実行し続ける - Linuxコマンド集：ITpro](http://itpro.nikkeibp.co.jp/article/COLUMN/20060227/230850/)