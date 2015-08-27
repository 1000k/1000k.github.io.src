---
title: '壁に穴を開けるプラグイン: Hole Punching Tool'
author: 1000k
layout: post
date: 2010-12-25
url: /2010/12/25/sketchup-plugin-hole-punchin-tool/
categories:
  - SketchUp
  - プラグイン
tags:
  - SketchUp
  - プラグイン
---
厚みのある壁に穴を開けるのは結構面倒です。壁面に平面を描いて[プッシュ/プル]ツールを使うだけですが、大量に同じような穴を開けるのは時間がかかります。また、一度開けた穴を塞ぐ場合も余計な線が残ってしまい、とにかく手間がかかります。

そんなとき、この「<a href="http://forums.sketchucation.com/viewtopic.php?f=323&t=30846" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://forums.sketchucation.com/viewtopic.php?f=323&t=30846', 'Hole Punching Tool']);" title="View topic - [Plugin] Hole Punching Tool v1.3 20101002 • SketchUcation Community Forums">Hole Punching Tool</a>」（穴開けツール）プラグインが役に立ちます。コンポーネントを壁面に配置し、右クリックから「Punch」を選ぶだけで、その形に壁が貫通します。

このプラグインが優れているのは穴を開けるだけでなく、元に戻すのも簡単ということです。穴自身とコンポーネントがリンクを保っており、「Undo Punch」をすることで穴が消えて元の壁に戻すことができます。
  
特に窓やドアを配置する際に有用です。

以下、使い方をメモします。

<!--more-->

## DL先

<a href="http://rhin.crai.archi.fr/RubyLibraryDepot/plugin_details.php?id=726" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://rhin.crai.archi.fr/RubyLibraryDepot/plugin_details.php?id=726', 'Hole Punching Tool (SU Plugin) &#8211; Ruby Library Depot']);" title="Hole Punching Tool (SU Plugin) - Ruby Library Depot">Hole Punching Tool (SU Plugin) &#8211; Ruby Library Depot</a>

## インストール方法

プラグインフォルダに HolePunchTool.rb を入れるだけです。

## 使い方

同梱されている HolePunchTool_documentation.txt に基本的な使い方が書いてあります。

たとえば以下のような窓を壁に配置し、窓の形に壁面に穴を開ける（パンチする）場合を考えます。

<a href="http://blog.1000k.net/wp-content/uploads/cutting_windows_1.jpg" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/cutting_windows_1.jpg', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/cutting_windows_1.jpg" alt="" title="hall_panching_tool_1" width="400" height="221" class="alignnone size-full wp-image-594" /></a>

壁面にコンポーネントを配置します。

<a href="http://blog.1000k.net/wp-content/uploads/hall_panching_tool_2.jpg" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/hall_panching_tool_2.jpg', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/hall_panching_tool_2.jpg" alt="" title="hall_panching_tool_2" width="400" height="221" class="alignnone size-full wp-image-595" /></a>

この時点ではもちろん穴は開いていません。

<a href="http://blog.1000k.net/wp-content/uploads/hall_panching_tool_3.jpg" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/hall_panching_tool_3.jpg', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/hall_panching_tool_3.jpg" alt="" title="hall_panching_tool_3" width="400" height="221" class="alignnone size-full wp-image-596" /></a>

窓を右クリックし、[Hole Punching&#8230;] > [Punch]を選択します。これで、壁に穴が開きました。
  
<a href="http://blog.1000k.net/wp-content/uploads/hall_panching_tool_5.jpg" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/hall_panching_tool_5.jpg', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/hall_panching_tool_5.jpg" alt="" title="hall_panching_tool_5" width="400" height="221" class="alignnone size-full wp-image-598" /></a>

<a href="http://blog.1000k.net/wp-content/uploads/hall_panching_tool_4.jpg" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/hall_panching_tool_4.jpg', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/hall_panching_tool_4.jpg" alt="" title="hall_panching_tool_4" width="400" height="221" class="alignnone size-full wp-image-597" /></a>

移動ツールを使うと、穴ごと移動するのがわかります。
  
<a href="http://blog.1000k.net/wp-content/uploads/hall_panching_tool_6.jpg" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/hall_panching_tool_6.jpg', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/hall_panching_tool_6.jpg" alt="" title="hall_panching_tool_6" width="400" height="221" class="alignnone size-full wp-image-599" /></a>

## 注意

うまくパンチできない場合、コンポーネントの軸または貼り付け平面が正しく設定されていない可能性があります。貼り付け平面が壁面と平行になるように設定しないと失敗するようです。

詳しい設定方法は以下の公式マニュアルを参照してください。（正直言って意味がわかりにくい説明ですが）

<a href="http://sketchup.google.com/support/bin/answer.py?hl=jp&answer=114526" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://sketchup.google.com/support/bin/answer.py?hl=jp&answer=114526', '[コンポーネントを作成] ダイアログ ボックス &#8211; SketchUp ヘルプ']);" title="[コンポーネントを作成] ダイアログ ボックス - SketchUp ヘルプ">[コンポーネントを作成] ダイアログ ボックス &#8211; SketchUp ヘルプ</a>

## チュートリアルムービー

以下のムービーではプラグインのダウンロード～Hole Punching方法を一通り紹介しています。



## 参考

<a href="http://forums.sketchucation.com/viewtopic.php?f=323&t=30846" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://forums.sketchucation.com/viewtopic.php?f=323&t=30846', 'View topic &#8211; [Plugin] Hole Punching Tool v1.3 20101002 • SketchUcation Community Forums']);" title="View topic - [Plugin] Hole Punching Tool v1.3 20101002 • SketchUcation Community Forums">View topic &#8211; [Plugin] Hole Punching Tool v1.3 20101002 • SketchUcation Community Forums</a>

プラグイン作成者のページ兼フォーラム。問題があった場合読んでみると参考になります。