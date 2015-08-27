---
title: jQMでアクセス中のページがトップページかどうか判別する
author: 1000k
layout: post
date: 2012-10-20
url: /2012/10/21/how-to-detect-the-page-is-homepage-or-not-in-jqm/
categories:
  - JavaScript
  - jQuery
tags:
  - jQuery
  - jQuery Mobile
---
現在アクセスしているURLがアプリのトップページか否かを判断するには、どうすればいいでしょうか？

jQuery Mobile では id 属性を使ってページを識別するため、簡単に判断できます。

<!--more-->

## やりたいこと

今アクセスしているURLが、アプリのトップページかどうか判別する関数を作る。

  * トップページなら true
  * それ以外なら false

## コード

トップページの id 属性が「top」という想定で進めます。

```
<div data-role="page" id="top" data-title="top page">
  <div data-role="header">

  </div>



  <div data-role="content">

  </div>


</div>
```


jQuery Mobile はページにアクセス時、画面を描画する前にページの構造を取得します。

つまり、今アクセスしているページの id 属性を取得できるので、これが top かそうでないかを判別すればOKです。

```
/**
 * アクセスしたページがトップページかどうか判別する
 *
 * @return {boolean} トップページなら true
 */
function isTop() {
    return $(":jqmData(role='page')").attr("id") === "top";
}
```


jQuery Mobile では data-* 属性を取得する時にはカスタムセレクタの jqmData() を使うのが作法なので、このような書き方になります。（下記の参考リンク内の「$.fn.jqmData()」参照）

## 参考

[jQuery Mobile Docs - Methods](http://jquerymobile.com/test/docs/api/methods.html)