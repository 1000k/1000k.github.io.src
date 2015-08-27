---
title: CSS3でスクロールしても背景のグラデーションは固定したままにする方法
author: 1000k
layout: post
date: 2011-02-24
url: /2011/02/24/fix-background-gradient-by-css3/
categories:
  - CSS
tags:
  - CSS
  - gradient
---
縦長のページをスクロールしても、背景のグラデーションは固定したままにする方法です。

実行サンプルは下記 URL で確認できます。

[Fixed Gradient Background - jsFiddle](http://jsfiddle.net/SWMME/1/)

<!--more-->

## HTML

```html
<div id="container">
  <h1>
    Fixed Gradient Background
  </h1>

</div>
```


## CSS

```css
h1 { color: #fff; }

#container {
    height: 2000px;
    background: #202a2e;
    background-image: -webkit-gradient(
        linear,
        left bottom,
        left top,
        color-stop(0, rgb(34,39,45)),
        color-stop(1, rgb(11,63,61))
    );
    background-image: -moz-linear-gradient(
        center bottom,
        rgb(34,39,45) 0%,
        rgb(11,63,61) 100%
    );
    background-attachment: fixed !important;
}
```


## 参考

  * [css - filter: gradient and background: fixed - Stack Overflow](http://stackoverflow.com/questions/5024143/filter-gradient-and-background-fixed)