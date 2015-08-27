---
title: XAMPPでBASIC認証を有効にする
author: 1000k
layout: post
date: 2012-07-08
url: /2012/07/08/enable-basic-auth-in-xampp/
categories:
  - Apache
  - 開発環境
tags:
  - Apache
  - XAMPP
  - トラブルシューティング
---
小一時間はまったのでメモ。

XAMPPPのApacheにBASIC認証をかけたのですが、何度やってもエラーログに下記のように「認証に失敗しました」というエラーが出ました。

```
[Sun Jul 08 16:51:31 2012] [error] [client ::1] user uso: authentication failure for "/mojamoja.php": Password Mismatch
```


「htpasswd 生成」などでググって出てくるようなツールで作ったパスワードではなぜかダメで、Apache付属の_htpasswd_を使わねばならないようです。

```
cd c:\xampp\apache\bin
htpasswd.exe -bc {パスワード生成先のパス} {ID} {パスワード}
```


こうして作成した .htpasswd なら正しく認証できました。

原因が今ひとつわからないですが、パスワードの暗号化方式がMD5でないといけないのかもしれません。

（上記のコマンドで生成されるパスワードはMD5）

## 参考

<a href="http://d.hatena.ne.jp/ise_daisuke/20080412/1207978885" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://d.hatena.ne.jp/ise_daisuke/20080412/1207978885', 'xamppでローカルでBasic認証 &#8211; 絶対に読んではいけない日記']);" >xamppでローカルでBasic認証 &#8211; 絶対に読んではいけない日記</a>