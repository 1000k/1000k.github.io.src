---
title: svnコマンドを打つ度に「パスワードを保存しますか？」と聞かれるのを抑制する
author: 1000k
layout: post
date: 2011-06-29
url: /2011/06/29/suppress-message-of-svn-command/
categories:
  - その他
  - 開発環境
tags:
  - Subversion
  - TIPS
---
Subversion 1.6にアップグレードしてからというもの、何かsvnコマンドを打つ度に下のメッセージが出るようになりました。

```
-----------------------------------------------------------------------
ATTENTION!  Your password for authentication realm:

<http://localhost:80> TEST SVN repository

can only be stored to disk unencrypted!  You are advised to configure
your system so that Subversion can store passwords encrypted, if
possible.  See the documentation for details.

You can avoid future appearances of this warning by setting the value
of the 'store-plaintext-passwords' option to either 'yes' or 'no' in
'/home/stylesen/.subversion/servers'.
-----------------------------------------------------------------------
暗号化されていないパスワードを保存しますか (yes/no)?
```


保存する気は無いのに毎回聞いてきてやかましいです。

抑止するには以下のように config ファイルを編集します。

```
# ~/.subversion/config

# [auth] ブロックに以下の行を追加する
store-passwords = no
store-plaintext-passwords = no
```


ちなみに、一度保存してしまったユーザーの情報は **~/.subversion/auth/svn.simple/**の中に保存されています。

セキュリティリスクになるので消してしまいましょう。

# 参考

  * [Subversion 1.6: Security Improvements Illustrated – LINUX For You Magazine](http://www.linuxforu.com/previews/subversion-16-security-improvements-illustrated/)
  * [Subversionのユーザ名とパスワードの記録場所 - No Programming, No Life](http://d.hatena.ne.jp/fumokmm/20070820/1187575030)