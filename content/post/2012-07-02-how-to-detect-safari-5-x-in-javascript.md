---
title: Safari 5.xのユーザーをJavaScriptで判別する
author: 1000k
layout: post
date: 2012-07-02
url: /2012/07/02/how-to-detect-safari-5-x-in-javascript/
categories:
  - JavaScript
tags:
  - Chrome
  - JavaScript
---
Safari 5.x系のみ発生する不具合があったため、JavaScriptで個別に分岐する必要がありました。

UserAgentがChromeと微妙に似ていて書きにくかったので、メモを残しておきます。

## やりたいこと

JavaScriptでSafari 5.x系のユーザーを判別する。

## スクリプト

```
window.onload = function() {
    var ua = navigator.userAgent.toLowerCase();
    var is_safari = ua.indexOf('safari') > -1 && ua.indexOf('version') > -1;
    var version = is_safari ? ua.match(/version\/([\d\.]+)/i)[1].substr(0,1) : null;

    if (is_safari && version == '5') {
        // Safari 5.xユーザー向けの処理
        alert('You use Safari 5.x');
    } else {
        // それ以外のユーザー向けの処理
    }
}
```


## 参考

  * [UserAgentString.com - List of User Agent Strings](http://www.useragentstring.com/pages/useragentstring.php)
      * 各ブラウザのUserAgent文字列の一覧。
      * [Safari](http://www.useragentstring.com/pages/Safari/)
      * [Chrome](http://www.useragentstring.com/pages/Chrome/)