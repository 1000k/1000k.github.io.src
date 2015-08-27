---
title: mod_rewriteでクエリ付きのURIをリダイレクトする
author: 1000k
layout: post
date: 2010-07-23
url: /2010/07/23/redirect-keeping-query-strings-with-mod-rewrite/
categories:
  - Apache
tags:
  - Apache
  - mod_rewrite
---
http://hige.jp/moja.php?category=1

→

http://hige.jp/moja.php?category=999

というリダイレクトをApacheで実現するにはどうすればいいでしょうか？リダイレクトの方法はいろいろありますが、今回RedirectやAliasディレクティブは使えません。これらはクエリ文字列（?category=999の部分）を把握できないようです。

そんな時は、mod\_rewriteの RewriteCond ${QUERY\_STRING} を使うと良いです。具体的には以下の用にします。

```
&lt;IfModule mod_rewrite.c>
    RewriteEngine on
    RewriteCond %{QUERY_STRING} category=1
    RewriteRule ^/moja.php /moja.php?category=999 [R=301,L]
&lt;/IfModule>
```


これでApacheを再起動すれば、リダイレクトが成功するようになっています。

### 参考

  * <a href="http://muumoo.jp/news/2006/04/06/0redirect.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://muumoo.jp/news/2006/04/06/0redirect.html', 'URIのリダイレクト設定をやってみた(管理人日記) &#8211; むぅもぉ.jp']);" title="URIのリダイレクト設定をやってみた(管理人日記) - むぅもぉ.jp">URIのリダイレクト設定をやってみた(管理人日記) &#8211; むぅもぉ.jp</a>
  * <a href="http://cubic9.com/Linux/Apache/%A5%E2%A5%B8%A5%E5%A1%BC%A5%EB/mod_rewrite/%B0%DC%C5%BE%A5%EA%A5%C0%A5%A4%A5%EC%A5%AF%A5%C8/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://cubic9.com/Linux/Apache/%A5%E2%A5%B8%A5%E5%A1%BC%A5%EB/mod_rewrite/%B0%DC%C5%BE%A5%EA%A5%C0%A5%A4%A5%EC%A5%AF%A5%C8/', 'Linux/Apache/モジュール/mod_rewrite/移転リダイレクト &#8211; cubic9.com']);" title="Linux/Apache/モジュール/mod_rewrite/移転リダイレクト - cubic9.com">Linux/Apache/モジュール/mod_rewrite/移転リダイレクト &#8211; cubic9.com</a>