---
title: CentOSでbzip2が解凍できないエラー
author: 1000k
layout: post
date: 2012-01-15
url: /2012/01/15/cannot-unzip-bzip2-in-centos/
categories:
  - Linux
  - コマンド
tags:
  - bzip2
  - CentOS
  - CloudCore
  - Linux
  - VPS
  - yum
  - トラブルシューティング
---
bzip2ファイルを解凍しようと思ったら、見慣れないエラーが出ました。

```
# tar xvf paco-2.0.9.tar.bz2
tar: bzip2: exec 不能: そのようなファイルやディレクトリはありません
tar: エラーを回復できません: 直ちに終了します
tar: Child returned status 2
tar: 処理中にエラーが起きましたが、最後まで処理してからエラー終了させました
```


ただ単にbzip2がインストールされていなかったのが原因でした。yumで入れて即解決です。

```
# yum -y install bzip2
...
Installed:
  bzip2.x86_64 0:1.0.3-6.el5_5

Complete!
```


CloudCore VPSでこのエラーに遭遇しました。

CentOSの最小構成でインストールされているようで、ごく普通のコマンドが使えないことが多いです。

パッケージが不足していないか注意が必要ですね。

# 参考

  * [2007-02-05 - プチ技術メモ](http://d.hatena.ne.jp/hiroponz/20070205)