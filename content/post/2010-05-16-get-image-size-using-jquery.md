---
title: '[JavaScript] 画像の実際のサイズを取得する (jQuery使用)'
author: 1000k
layout: post
date: 2010-05-16
url: /2010/05/16/get-image-size-using-jquery/
categories:
  - JavaScript
tags:
  - JavaScript
  - jQuery
---
  * imgタグのwidth/height要素を指定していない場合、イメージがどう表示されるかはブラウザによって異なる。
  * すべてのブラウザで画像オリジナルのwidth/heightを取得しようとしても、ブラウザ毎にパラメータやメソッドが異なる
  * jQueryを利用すると、下記コードで画像の実際のサイズを取得できる

```
/*
 * 画像の実際のサイズを取得する
 *
 * @param  string  image  img要素
 */
function getActualDimension(image) {
    var run, mem, w, h, key = "actual";

    // for Firefox, Safari, Google Chrome
    if ("naturalWidth" in image) {
        return {width: image.naturalWidth, height: image.naturalHeight};
    }
    if ("src" in image) { // HTMLImageElement
        if (image[key] &#038;&#038; image[key].src === image.src) {return  image[key];}

        if (document.uniqueID) { // for IE
            w = $(image).css("width");
            h = $(image).css("height");
        } else { // for Opera and Other
            mem = {w: image.width, h: image.height}; // keep current style
            $(this).removeAttr("width").removeAttr("height").css({width:"",  height:""});    // Remove attributes in case img-element has set width  and height (for webkit browsers)
            w = image.width;
            h = image.height;
            image.width  = mem.w; // restore
            image.height = mem.h;
        }
        return image[key] = {width: w, height: h, src: image.src}; // bond
    }

    // HTMLCanvasElement
    return {width: image.width, height: image.height};
}
```
