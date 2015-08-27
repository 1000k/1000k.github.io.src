---
title: NetBeansの日本語出力が文字化けするときの対処法
author: 1000k
layout: post
date: 2011-01-22
url: /2011/01/22/fixing-gable-in-netbeans/
categories:
  - その他
  - 開発環境
tags:
  - NetBeans
  - トラブルシューティング
  - 文字化け
---
NetBeansで出力ウインドウの日本語が文字化けしてしまった時の解決方法です。

### 検証環境

NetBeans IDE 7.0 Beta

### 対処法

**netbeans.conf**の**「netbeans\_default\_options」**ブロックに**「-J-Dfile.encoding=UTF-8」**を追加してやればいいようです。

下記のようになります。

```
// C:\Program Files\NetBeans 7.0 Beta\etc\netbeans.conf

# Options used by NetBeans launcher by default, can be overridden by explicit
# command line switches:
netbeans_default_options="-J-client -J-Xss2m -J-Xms32m -J-XX:PermSize=32m -J-XX:MaxPermSize=384m -J-Dnetbeans.logger.console=true -J-ea -J-Dapple.laf.useScreenMenuBar=true -J-Dapple.awt.graphics.UseQuartz=true -J-Dsun.java2d.noddraw=true -J-Dfile.encoding=UTF-8"
```


結果

[{{< img src="/img/LerningRuby-N_2059-e1295677532145-300x138.png" title="" >}}](http://blog.1000k.net/wp-content/uploads/LerningRuby-N_2059-e1295677532145.png)

### 日本語フォントを指定する (2011/07/07追記)

まだ文字化けする場合、出力ウインドウのフォントが日本語に対応していない可能性があります。出力ウインドウの適当な場所を**右クリック -> [フォントを選択…]**で、日本語フォントを指定してください。

## 参考

[Complete Mirage - NetBeansでJRuby on Rails](http://completemirage.blog55.fc2.com/blog-entry-39.html)