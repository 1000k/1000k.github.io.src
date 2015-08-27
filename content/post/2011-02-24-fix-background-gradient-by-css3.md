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

<a href="http://jsfiddle.net/SWMME/1/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://jsfiddle.net/SWMME/1/', 'Fixed Gradient Background &#8211; jsFiddle']);" >Fixed Gradient Background &#8211; jsFiddle</a>

<!--more-->

## HTML

```
<div id="container">
  <h1>
    Fixed Gradient Background
  </h1>

</div>
```


## CSS

```
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

  * <a href="http://stackoverflow.com/questions/5024143/filter-gradient-and-background-fixed" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://stackoverflow.com/questions/5024143/filter-gradient-and-background-fixed', 'css &#8211; filter: gradient and background: fixed &#8211; Stack Overflow']);" >css &#8211; filter: gradient and background: fixed &#8211; Stack Overflow</a>