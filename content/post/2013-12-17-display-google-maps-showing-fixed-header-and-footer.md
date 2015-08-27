---
title: ヘッダーとフッターを固定しつつ高さ100% の Google Maps を描画する方法
author: 1000k
layout: post
date: 2013-12-17
url: /2013/12/17/display-google-maps-showing-fixed-header-and-footer/
categories:
  - CSS
  - JavaScript
tags:
  - Google Maps
  - TIPS
---
<a href="http://jsfiddle.net/893s2/2/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://jsfiddle.net/893s2/2/', 'このデモ']);" >このデモ</a>を見れば、何をやりたいかすぐ把握できると思います。

<a href="http://blog.1000k.net/wp-content/uploads/map_1.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/map_1.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/map_1-300x225.png" alt="map_1" width="300" height="225" class="alignnone size-medium wp-image-1683" /></a>

<a href="http://blog.1000k.net/wp-content/uploads/map_2.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/map_2.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/map_2-190x300.png" alt="map_2" width="190" height="300" class="alignnone size-medium wp-image-1684" /></a>

ウィンドウの幅や高さを変えても、ヘッダーとフッターは常に同じ高さのままで、マップの部分はウィンドウサイズに合うように伸縮しています。

以下、このようなコードの実装方法を書きます。

<!--more-->

## コード

まずはお決まりの Google Maps 呼び出しコードと、HTML の構造です。

```







<div id="container">
  &lt;header>header&lt;/header>



  <div id="content">
    <div id="map_canvas" style="width: 100%; height: 100%;">

    </div>

  </div>

      &lt;footer>This footer is absolutely positioned&lt;/footer>

</div>

<!-- /#container -->


```


これについては説明は不要でしょう。ページ全体を覆う `#container` の中に、ヘッダーとコンテンツとフッターがあります。

次に、ポイントとなる CSS は以下のとおりです。

```
html,body {
  margin: 0;
  padding: 0;
  height: 100%; /* needed for container min-height */
  background: #300;
}

#container {
  background: #ccc;
  height: 100%;
  margin: 0 auto;
  min-height: 100%;
  overflow: hidden;
  position: relative;
  width: 100%;

  /*height:auto !important;  real browsers */
  height:100%; /* IE6: treaded as min-height*/

  min-height:100%; /* real browsers */
}

header {
  height: 108px;
  position: relative;
  background: #0ff;
}

#content{
  position: absolute;
  top: 0;
  bottom: 0;
  overflow: hidden;
  margin-top: 108px;    /* header's height */
  width: 100%;
}

footer {
  position: absolute;
  clear: both;
  width: 100%;
  height: 40px;
  bottom: 0;        /* stick to bottom */
  background: #ff0;
}
```


全体を覆う `#container` には `min-height: 100%;` を指定し、ブラウザの高さいっぱいに描画するようにしています。また、`position: relative;` を指定することで、その内部で自由に要素を配置できるようにしています。

`header` に関しては特にコメントは不要ですね。ただ高さを指定しているだけです。

`#content` にはいくつか工夫があります。まず、`position: absolute; top: 0; bottom: 0;` によって、要素の高さを 100% にしています。次に、`margin-top: 108px;` というように header と同じ高さでマージンを作ることで、ヘッダーとマップを隙間なく描画しています。

最後に `footer` ですが、`position: absolute; bottom: 0;` によって、常に `#container` の最下部に貼り付くようにしています。

実現したいことはシンプルですが、実装するのに結構工夫が必要だったのでメモしておきました。

## 参考

  * <a href="http://stackoverflow.com/questions/2821596/100-height-with-fixed-footer-and-embedded-google-map" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://stackoverflow.com/questions/2821596/100-height-with-fixed-footer-and-embedded-google-map', 'html &#8211; 100% height with fixed footer and embedded Google Map &#8211; Stack Overflow']);" >html &#8211; 100% height with fixed footer and embedded Google Map &#8211; Stack Overflow</a>
      * 今回のデモは、この Q&A に書かれているコードをシンプルにしただけです。