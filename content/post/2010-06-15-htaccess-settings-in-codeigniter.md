---
title: CodeIgniterで使う.htaccessの設定
author: 1000k
layout: post
date: 2010-06-15
url: /2010/06/15/htaccess-settings-in-codeigniter/
categories:
  - CodeIgniter
  - PHP
tags:
  - .htaccess
  - CodeIgniter
  - 備忘録
---
```
RewriteEngine on
RewriteCond $1 !^(index\.php|css|user_guide|.+\.gif$|.+\.jpe?g$|.+\.png$|.+\.js$|.+\.css$)
RewriteRule ^(.*)$ /index.php/$1 [L]
```


例えば 「http://localhost/hige/moja」がトップディレクトリ(=index.phpの置いてある場所)の時、

```
RewriteEngine on
RewriteCond $1 !^(index\.php|css|user_guide|.+\.gif$|.+\.jpe?g$|.+\.png$|.+\.js$|.+\.css$)
RewriteRule ^(.*)$ /hige/moja/index.php/$1 [L]
```
