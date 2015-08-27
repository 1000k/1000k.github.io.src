---
title: '[jQuery] セレクタで「<」「>」が選択できない'
author: 1000k
layout: post
date: 2010-05-16
url: /2010/05/16/cannot-select-gt-and-lt-with-jquery/
categories:
  - JavaScript
tags:
  - JavaScript
  - jQuery
---
# 症状

JavaScriptにおいて、「<」「>」は基本的にダメ文字（使用不可）だが、jQueryのセレクタ内で使用すると選択できたりできなかったりすることがある。

# 原因

そもそも、XHTMLの仕様では「<」「>」は属性値に使ってはいけない。

たまに仕様で許可されてない記号も使えるが、そうしたものが選択できるのはブラウザ依存の偶然でしかない。

# 対策

こういう文字を使うときは、別の文字に置換でもしないとダメ。例えばDOMロード後に全角の「＜」「＞」に置換するとか。

# 参考

<a href="http://forum.jquery.com/topic/selector-problem-foo-and-bar-works-but-buz-doesn-t-12-4-2010" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://forum.jquery.com/topic/selector-problem-foo-and-bar-works-but-buz-doesn-t-12-4-2010', '\nSelector problem: $(\'#&#092;<foo\') and $(\'#bar&#092;>\') works, but $(\'#&#092;<buz&#092;>\') doesn\'t. &#8211; jQuery Forum']);" ><br /> Selector problem: $('#&#92;<foo') and $('#bar&#92;>') works, but $('#&#92;<buz&#92;>') doesn't. &#8211; jQuery Forum</a>