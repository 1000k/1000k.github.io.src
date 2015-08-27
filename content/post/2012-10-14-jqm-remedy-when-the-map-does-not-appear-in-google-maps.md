---
title: jQM + Google Maps でマップが表示されない時の対処法
author: 1000k
layout: post
date: 2012-10-14
url: /2012/10/14/jqm-remedy-when-the-map-does-not-appear-in-google-maps/
categories:
  - JavaScript
  - jQuery
tags:
  - Google Maps
  - jQuery Mobile
  - トラブルシューティング
---
jQuery Mobile 1.2.0 Final での不具合です。

jQuery Mobile と Google Maps API の相性がいまいち良くないようで、いろいろな所で不具合が出ます。

今回発見したのは、フォーム画面から戻ってくるとマップが表示されない問題です。

  1. マップが設置されたページからフォーム画面へ遷移する
  2. フォームの値をPOSTして元のページへ戻る
  3. マップが表示されない

Google Maps と統合するサービスには致命的になりかねない不具合です。

以下に不具合の検証用コードと、かなり妥協した解決案を記します。

<!--more-->

## 検証用コード

### gmap1.html

マップを表示するページです。

```







<div data-role="page" data-theme="a" id="page1">
  <div data-role="content">
    <div id="map_canvas">

    </div>


    <a href="gmap2.html" data-role="button" data-rel="dialog">Go to gmap2.html</a>

  </div>

</div>


```


### gmap2.html

フォーム画面。今回は dialog を使いました。

```






<div data-role="dialog" data-theme="a" id="page2">
  </form>

          <a href="gmap1.html" data-role="button" data-rel="back">back to gmap1.html</a>

</div>


```


これらのページを設置し、「gmap1.html -> gmap2.html -> gmap1.html」という遷移をすると、2回目の gmap1.html でマップが表示されません。

## 解決案

jQuery Mobile が内部で Ajax をガチャガチャやっているのが悪影響を及ぼしているようなので、Ajaxをオフにすれば治りました。

上記のコードで、「// $.mobile.ajaxEnabled = false;」となっている部分をコメントアウトすればOKです。

### gmap1.html

```
$.mobile.ajaxEnabled = false;
```


### gmap2.html

```
$.mobile.ajaxEnabled = false;
```


この対応によりマップは表示されるようになりますが、画面遷移時のエフェクトを始めとしたもろもろの機能が使えなくなります。

非常に心苦しい対応ですが、今のところこれぐらいしか思いつきませんでした。

なお、全ページにこの1行を書くのは面倒なので、外部JSファイルに以下の指定をして全ページのhead内で読み込むようにすれば楽です。

```
$(document).bind("pageinit", function() {
  $.mobile.ajaxEnabled = false;
});
```


## 参考

  * [jQuery Mobile Docs - Configuring default settings](http://jquerymobile.com/test/docs/api/globalconfig.html)
      * $.mobile.ajaxEnabled のリファレンス。
  * [jquery_ui_map_v_3_tutorial - jquery-ui-map - Google map v3 plugin for jQuery and jQuery Mobile - Google Project Hosting](http://code.google.com/p/jquery-ui-map/wiki/jquery_ui_map_v_3_tutorial)
      * 「Help, my map is rendered incorrectly! (jQM)」に同じ不具合の対処法が書いてありました。