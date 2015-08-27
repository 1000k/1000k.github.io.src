---
title: 「要素をクリックする」命令をjQueryで書く
author: 1000k
layout: post
date: 2011-06-22
url: /2011/06/22/click-specific-element-by-jquery/
categories:
  - JavaScript
  - jQuery
tags:
  - jQuery
  - TIPS
---
LightBoxのように、リンクをクリックするとアクションが発生するスクリプトは数多く存在します。

任意の場所をクリックした時にそのアクションを起動したい場合は、jQueryの**click()**メソッドを使うと簡単に実現できます。

# 例

classが"uso"のaリンクをクリックした時に "a#mojamoja" をクリックした時のアクション起動したい場合は、下記のように書けます。

```javascript
$(function() {
    $("a.uso").click(function(e) {
        e.preventDefault();
        $("a#mojamoja").eq(0).click();
    });
});
```


# 応用（悪用？）

FirebugやChromeのスクリプト入力ボックスに下記のコードを書き込むと、button#usoを永遠に毎秒10回叩き続けます。

```
setInterval(function() {$("button#uso").eq(0).click();}, 100);
```

「どうしてもボタンが連打したい。でも指が折れて動かせない…。」という名人にお勧めです。

# 参考

  * [click() - jQuery 日本語リファレンス](http://semooh.jp/jquery/api/events/click/_/)
  * [click(fn) - jQuery 日本語リファレンス](http://semooh.jp/jquery/api/events/click/fn/)
  * [JavaScript/jQuery/readyじゃなくてonloadイベントに関数を追加したい - TOBY SOFT wiki](http://tobysoft.net/wiki/index.php?JavaScript%2FjQuery%2Fready%A4%B8%A4%E3%A4%CA%A4%AF%A4%C6onload%A5%A4%A5%D9%A5%F3%A5%C8%A4%CB%B4%D8%BF%F4%A4%F2%C4%C9%B2%C3%A4%B7%A4%BF%A4%A4)