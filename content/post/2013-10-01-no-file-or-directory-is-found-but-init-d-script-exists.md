---
title: init.dスクリプトが存在するのに「そのようなファイルやディレクトリはありません」と言われた
author: 1000k
layout: post
date: 2013-10-01
url: /2013/10/01/no-file-or-directory-is-found-but-init-d-script-exists/
categories:
  - Linux
tags:
  - CentOS
  - Vim
  - トラブルシューティング
---
CentOS 6.4 でとある init.d スクリプトを実行したら以下のようなエラーが出ました。

```
# service mysql_master
env: /etc/init.d/mysql_master: そのようなファイルやディレクトリはありません
```


いえ、何度確認してもそのファイルは確かに存在しています。

ファイルの中身を見ても何も異常が見つかりません。

グーグル先生に道を伺ったところ、「改行コードがLFじゃないから」とのこと。

さっそく改行コードを確認したところ CR+LF でした。

Vim で以下のコマンドを実行したら、正しく実行されるようになりました。

```
:set ff=unix
:wq
```


```
# service mysql_master
Usage: /etc/init.d/mysql_master {start|stop|restart|status}
```


## 参考

  * [init.d script error bad interpreter](http://www.linuxquestions.org/questions/red-hat-31/init-d-script-error-bad-interpreter-204902/)
  * [Convert DOS line endings to Linux line endings in vim - Stack Overflow](http://stackoverflow.com/questions/82726/convert-dos-line-endings-to-linux-line-endings-in-vim)