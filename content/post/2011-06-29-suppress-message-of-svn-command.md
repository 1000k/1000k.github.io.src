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

&lt;http://localhost:80> TEST SVN repository

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

  * <a href="http://www.linuxforu.com/previews/subversion-16-security-improvements-illustrated/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.linuxforu.com/previews/subversion-16-security-improvements-illustrated/', 'Subversion 1.6: Security Improvements Illustrated – LINUX For You Magazine']);" title="Subversion 1.6: Security Improvements Illustrated – LINUX For You Magazine">Subversion 1.6: Security Improvements Illustrated – LINUX For You Magazine</a>
  * <a href="http://d.hatena.ne.jp/fumokmm/20070820/1187575030" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://d.hatena.ne.jp/fumokmm/20070820/1187575030', 'Subversionのユーザ名とパスワードの記録場所 &#8211; No Programming, No Life']);" title="Subversionのユーザ名とパスワードの記録場所 - No Programming, No Life">Subversionのユーザ名とパスワードの記録場所 &#8211; No Programming, No Life</a>