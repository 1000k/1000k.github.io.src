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
    var is_safari = ua.indexOf('safari') > -1 &#038;&#038; ua.indexOf('version') > -1;
    var version = is_safari ? ua.match(/version\/([\d\.]+)/i)[1].substr(0,1) : null;

    if (is_safari &#038;&#038; version == '5') {
        // Safari 5.xユーザー向けの処理
        alert('You use Safari 5.x');
    } else {
        // それ以外のユーザー向けの処理
    }
}
```


## 参考

  * <a href="http://www.useragentstring.com/pages/useragentstring.php" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.useragentstring.com/pages/useragentstring.php', 'UserAgentString.com &#8211; List of User Agent Strings']);" title="UserAgentString.com - List of User Agent Strings">UserAgentString.com &#8211; List of User Agent Strings</a>
      * 各ブラウザのUserAgent文字列の一覧。
      * <a href="http://www.useragentstring.com/pages/Safari/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.useragentstring.com/pages/Safari/', 'Safari']);" title="UserAgentString.com - List of Safari User Agent Strings">Safari</a>
      * <a href="http://www.useragentstring.com/pages/Chrome/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.useragentstring.com/pages/Chrome/', 'Chrome']);" title="UserAgentString.com - List of Chrome User Agent Strings">Chrome</a>