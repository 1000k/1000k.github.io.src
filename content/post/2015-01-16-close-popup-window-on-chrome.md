---
title: JavaScript で Chrome のウィンドウを閉じる方法
author: 1000k
layout: post
date: 2015-01-16
url: /2015/01/16/close-popup-window-on-chrome/
categories:
  - JavaScript
tags:
  - JavaScript
---
以前リリースしたあるサービスで、「一部の入力フォームを別ウィンドウをポップアップして入力させ、終わったらリンクをクリックして閉じる」という JavaScript の処理を入れていたのですが、なぜか最近の Chrome で画面が固まってしまう不具合が発生しました。2014年の春にテストした時は問題なく動いていたのですが。

## 再現方法

不具合は以下の流れで発生します。確認したブラウザは 39.0.2171.99m です。

  1. 親ウィンドウにある `<a onClick="window.open()">` リンクを叩き、子ウィンドウをポップアップさせる。
  2. 子ウィンドウにある `<a onClick="window.open('about:blank','_self').close()">` リンクを叩く。
  3. 親・子ウィンドウ両方が固まってしまう。

ウィンドウを閉じる時に単純な `window.close()` ではなく `window.open('about:blank','_self').close()` を使っているのは、IE/FF/Chrome いずれのブラウザーでも綺麗に閉じるための有名な Hack だったからです。詳細は <a href="http://kojikoji75.hatenablog.com/entry/2013/12/15/223839" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://kojikoji75.hatenablog.com/entry/2013/12/15/223839', 'JavaScriptでWindow.closeする時のブラウザ別対応まとめ - TechNote']);" >JavaScriptでWindow.closeする時のブラウザ別対応まとめ - TechNote</a> がわかりやすいです。

どうやらブラウザーのバージョンアップに伴い、このあたりの事情が変わってきてたようです。

今なら子ウィンドウを消す JavaScript をどう実装するべきなのかを、少し検証してみました。

<!--more-->

## 検証

以下の2つの HTML を用意します。

popup_parent.html

```html
<html lang="en">
<head>
    <title>Parent</title>
</head>
<body>
    <p><a href="#" onClick="window.open('popup_child.html', 'child', 'width=300,height=300');">Open popup window</a></p>
</body>
</html>
```

popup_child.html

```
<html lang="en">
<head>
    <title>Child</title>
</head>
<body>
    <p><a href="#" onClick="window.open('about:blank', '_self').close()">Close window 1</a></p>
    <p><a href="#" onClick="window.close();">Close window 2</a></p>
</body>
</html>
```

検証手順は以下の通り。

  1. popup_parent.html にアクセスし、`Open popup window` リンクを叩く。
  2. 子ウィンドウが開くので、`Close window 1` (ハック版) と `Close window 2` (単純版) の2つのリンクをそれぞれ叩き、正しく閉じられるかどうかを記録する。

検証結果は次のようになりました。「o」は閉じられた場合、「x」は閉じられなかった場合です。

| ブラウザー                | Close window 1 | Close window 2 |
| -------------------- | -------------- | -------------- |
| IE 10                | o              | o              |
| Chrome 39.0.2171.99m | x              | o              |
| FireFox 34.0.5       | o              | o              |
| Safari 5.1.7         | o              | o              |

Chrome だけハック版がうまく動かないという結果になりました。

悪さをしているのは WebKit か検証するため、一応 Safari でも実験しましたが、問題はありませんでした。今のところは Chrome だけ処理を分けるようにすれば良さそうです。ただ、Safari の WebKit は若干バージョンが古い (Safari = WebKit/534.57.2, Chrome = Webkit/537.36) ので、今後のバージョンアップでどうなるかは不明です。

## 解決策

検証結果から、全て Close window 2 (単純版) の書き方にすれば解決するように見えますが、一応古いブラウザーとの互換性を考えて、Chrome だけ単純版に分岐するような JS にしました。

```
<p><a href="#" onClick="if (/Chrome/i.test(navigator.userAgent)) { window.close(); } else { window.open('about:blank', '_self').close(); }">Close window 3</a></p>
```

これでどのブラウザーでも閉じられるようになりました。

## 参考

  * <a href="http://stackoverflow.com/questions/19761241/window-close-and-self-close-do-not-close-the-window-in-chrome" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://stackoverflow.com/questions/19761241/window-close-and-self-close-do-not-close-the-window-in-chrome', 'javascript - window.close and self.close do not close the window in Chrome - Stack Overflow']);" >javascript - window.close and self.close do not close the window in Chrome - Stack Overflow</a>
  * <a href="http://stackoverflow.com/questions/12625876/how-to-detect-chrome-and-safari-browser-webkit" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://stackoverflow.com/questions/12625876/how-to-detect-chrome-and-safari-browser-webkit', 'javascript - How to detect chrome and safari browser (webkit) - Stack Overflow']);" >javascript - How to detect chrome and safari browser (webkit) - Stack Overflow</a>
  * <a href="http://kojikoji75.hatenablog.com/entry/2013/12/15/223839" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://kojikoji75.hatenablog.com/entry/2013/12/15/223839', 'JavaScriptでWindow.closeする時のブラウザ別対応まとめ - TechNote']);" >JavaScriptでWindow.closeする時のブラウザ別対応まとめ - TechNote</a>