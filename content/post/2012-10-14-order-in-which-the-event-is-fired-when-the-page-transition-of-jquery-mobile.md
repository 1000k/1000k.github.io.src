---
title: jQuery Mobileのページ遷移時にイベントが発火する順番
author: 1000k
layout: post
date: 2012-10-13
url: /2012/10/14/order-in-which-the-event-is-fired-when-the-page-transition-of-jquery-mobile/
categories:
  - JavaScript
  - jQuery
tags:
  - jQuery Mobile
  - 検証
---
ページ遷移持のイベントの発火タイミングをよくよく理解しておかなければ、jQuery Mobile を使いこなすことはできません。

単純なページ間の遷移で発生するイベントの順番を確認しておきます。

なお、イベント一覧は [jQuery Mobile Docs - Events](http://jquerymobile.com/demos/1.2.0/docs/api/events.html) に記されています。

<!--more-->

## 検証コード

コードはgistにも公開してあります。→[Verify the event firing timings of jQuery Mobile — Gist](https://gist.github.com/3886193)

### page1.html

```






<div data-role="page" data-theme="a" id="page1">
  <div data-role="content">
    <a href="page2.html" data-role="button">Go to page2.html</a>

  </div>

</div>


```


### page2.html

page1 とほぼ同じなので、[data-role="page"]ブロック内だけ。

```
<div data-role="page" data-theme="e" id="page2">
  <div data-role="content">
    <a href="page1.html" data-role="button">Back to page1.html</a>

  </div>

</div>
```


### transition.js

イベント発火時にコンソールにイベント名が出るようにしています。

```
// Page load events
$(document).bind("pagebeforeload", function(e, data) {
  console.log("pagebeforeload: " + data.url);
});
$(document).bind("pageload", function(e, data) {
  console.log("pageload: " + data.url);
});
$(document).bind("pageloadfailed", function() { console.log("pageloadfailed"); });

// Page change events
$(document).bind("pagebeforechange", function() { console.log("pagebeforechange"); });
$(document).bind("pagechange", function() { console.log("pagechange"); });
$(document).bind("pagechangefailed", function() { console.log("pagechangefailed"); });

// Page transition events
$(document).bind("pagebeforeshow", function() { console.log("pagebeforeshow"); });
$(document).bind("pagebeforehide", function() { console.log("pagebeforehide"); });
$(document).bind("pageshow", function() { console.log("pageshow"); });
$(document).bind("pagehide", function() { console.log("pagehide"); });

// Page initialization events
$(document).bind("pagebeforecreate", function() { console.log("pagebeforecreate"); });
$(document).bind("pagecreate", function() { console.log("pagecreate"); });
$(document).bind("pageinit", function() { console.log("pageinit"); });

// Page remove events
$(document).bind("pageremove", function() { console.log("pageremove"); });
```


## 実験

「page1.html -> page2.html -> page1.html -> page2.html」という遷移をした時のコンソールログは以下です。

```
// page1.html にアクセス
pagebeforechange
pagebeforecreate
pagecreate
pageinit
pagebeforeshow
pageshow
pagechange

// 「Go to page2.html」をクリック
pagebeforechange
pagebeforeload: http://localhost/page2.html
pagebeforecreate
pagecreate
pageinit
pageload: http://localhost/page2.html
pagebeforechange
pagebeforehide
pagebeforeshow
pagehide
pageshow
pagechange

// 「Back to page1.html」をクリック
pagebeforechange
pagebeforechange
pagebeforehide
pagebeforeshow
pageremove
pagehide
pageshow
pagechange

// もう一度「Go to page2.html」をクリック
pagebeforechange
pagebeforeload: http://localhost/page2.html
pagebeforecreate
pagecreate
pageinit
pageload: http://localhost/page2.html
pagebeforechange
pagebeforehide
pagebeforeshow
pagehide
pageshow
pagechange
```


## 考察

気付いた点をメモします。

  * pagechange は全ての遷移の最後で発生する。
  * page2.html に遷移するたびに pageinit が発火している。
  * 逆に、page1.html で pageinit が発火するのは最初のアクセス時のみ。

## 参考

  * [jQuery Mobile Docs - Scripting pages](http://jquerymobile.com/demos/1.2.0/docs/pages/page-scripting.html)
  * [jQuery Mobile Docs - Events](http://jquerymobile.com/demos/1.2.0/docs/api/events.html)
  * [はじめての jQuery Mobile 1.0 ～その2：ページ遷移～ - etc9](http://d.hatena.ne.jp/Naotsugu/20120103/1325568080)