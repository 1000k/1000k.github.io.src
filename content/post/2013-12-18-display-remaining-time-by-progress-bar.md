---
title: 残り時間をプログレスバーで表示するチュートリアル
author: 1000k
layout: post
date: 2013-12-18
url: /2013/12/18/display-remaining-time-by-progress-bar/
categories:
  - HTML5
  - jQuery
tags:
  - CSS
  - HTML5
  - JavaScript
  - jQuery
  - チュートリアル
---
[E.gg Timer](http://e.ggtimer.com/) のようなタイマーアプリを作りたかったので、プログレスバーの作り方を調べました。タイマーそのものより、progress タグのスタイルシートをクロスブラウザ対応させるのに苦労しました。

今回作ったのは下の画像のような簡単なタイマーです。経過時間に合わせてプログレスバーが赤くなります。

{{< img src="/img/timer.png" title="timer" >}}

[デモはこちら。](http://jsfiddle.net/tcxx9/3/)

なお、IE11, Firefox26, Chrome31 で動作を確認しています。

<!--more-->

## 実装コード

### HTML

プログレスバー、時間入力欄、開始/停止ボタンがあるだけです。

```
<div class="progressWrapper">
  <progress id="bar" value="0" max="1000" min="0"></progress>

</div>



<fieldset>
  Time (msec): <input type="text" value="5000" id="time" />

    <button id ="start">start</button>
    <button id ="stop">stop</button>

</fieldset>
```


### JavaScript

jQuery 1.10.2 で試しました。経過時間に合わせて progress タグの value 要素を増やしていくだけです。

```
var timer,
  limitMs = 0,
  restMs = 0,
  resolutionMs = 50,    /* NOTE: Too small value does not work on IE11. */
  maxBar;

var countdown = function() {
  restMs -= resolutionMs;

  var restRate = (limitMs - restMs) / limitMs;
  var restBarLength = maxBar * restRate

  $('#bar').attr('value', restBarLength);

  if (restMs < 0) {
    resetTimer();
    alert('time expired');
  }
};

var resetTimer = function() {
  clearInterval(timer);
  limitMs = restMs = $('#time').val();
  $('#bar').attr('value', 0);
};

$(function() {
  maxBar = $('#bar').attr('max');

  $('#start').on('click', function() {
    resetTimer();
    timer = setInterval('countdown()', resolutionMs);
  });

  $('#stop').on('click', function() {
    resetTimer();
  });
});
```


ひとつだけ IE11 でバグがありました。`resolutionMs` (タイマーを更新する間隔) を10ミリ秒以下にすると、赤いバーが全く表示されなくなりました。50ミリ秒ならなんとか描画されました。

どうも IE11 ではバーの表示を勝手に加速度を付けて滑らかにする機能が付いているのですが、これが悪さをしているようです。デバッグしてみると value 属性は間違いなく増加しているのですが、画面には反映されませんでした。IE ユーザーはご注意ください。

### CSS

progress 要素はブラウザ毎に属性の解釈が結構異なります。Opera は面倒なので試してません。すいません。

```
.progressWrapper {
  width: 100%;
}

progress {
  /* Turn off default styling. */
  appearance: none;
  -moz-appearance: none;
  -webkit-appearance: none;
  border: 0;

  height: 10px;
  width: 100%;
  color: red;       /* IE */
  background: navy; /* Firefox */
}

/* Chrome needs '-webkit-progress-value' and '-webkit-progress-bar' attributes. */
progress::-webkit-progress-value {
  background: red;
}

progress::-webkit-progress-bar {
  background: navy;
}

/* Firefox needs only '-moz-progress-bar' attiribute. */
progress::-moz-progress-bar {
  background: red;
}
```


## 参考

  * [Creating & Styling Progress Bar With HTML5](http://www.hongkiat.com/blog/html5-progress-bar/)
  * [カウントダウンタイマー－JavaScript入門](http://www.pori2.net/js/timer/6.html)
  * [Cross Browser HTML5 Progress Bars In Depth](http://www.useragentman.com/blog/2012/01/03/cross-browser-html5-progress-bars-in-depth/)
      * カウントダウンに合わせて動くサルのアニメや、スピードメーターの書き方など。
  * [The HTML5 progress Element | CSS-Tricks](http://css-tricks.com/html5-progress-element/)
  * [::-moz-progress-bar - CSS | MDN](https://developer.mozilla.org/ja/docs/Web/CSS/::-moz-progress-bar)