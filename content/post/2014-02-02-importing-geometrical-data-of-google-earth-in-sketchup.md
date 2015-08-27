---
title: Sketchup で Google Earth の地形データをインポートする手順
author: 1000k
layout: post
date: 2014-02-02
url: /2014/02/02/importing-geometrical-data-of-google-earth-in-sketchup/
categories:
  - SketchUp
tags:
  - SketchUp
  - チュートリアル
---
Google Earth の 3D の地形データを Sketchup にインポートするチュートリアルです。

<!--more-->

## 手順

### 場所のインポート

&#8220;ファイル > ジオロケーション > 場所を追加&#8221; をクリック。

<a href="http://blog.1000k.net/wp-content/uploads/su_tutorial_001.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/su_tutorial_001.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/su_tutorial_001.png" alt="su_tutorial_001" width="550" height="358" class="alignnone size-full wp-image-1817" /></a>

Google Maps が開くので、地名を検索する。

&#8220;地域を選択&#8221; をクリック。

矩形の範囲を設定し、&#8221;グラブ&#8221; をクリック。

<a href="http://blog.1000k.net/wp-content/uploads/su_tutorial_002.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/su_tutorial_002.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/su_tutorial_002-300x224.png" alt="su_tutorial_002" width="300" height="224" class="alignnone size-medium wp-image-1819" /></a>

これで地形データが Sketchup にインポートされます。

<a href="http://blog.1000k.net/wp-content/uploads/su_tutorial_003.jpg" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/su_tutorial_003.jpg', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/su_tutorial_003-1024x680.jpg" alt="su_tutorial_003" width="474" height="314" class="alignnone size-large wp-image-1818" /></a>

### 3D モデルを表示する

この時点では 2D の画像しかありませんが、3D のデータも非表示のレイヤーで取り込まれています。
  
表示するには次の手順に従います。

  1. &#8220;ウィンドウ > レイヤ&#8221; をクリックして、レイヤパネルを開く。
  2. &#8220;Google Earth Terrain&#8221; の &#8220;可視&#8221; チェックボックスをクリック。

<a href="http://blog.1000k.net/wp-content/uploads/su_tutorial_004.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/su_tutorial_004.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/su_tutorial_004.png" alt="su_tutorial_004" width="248" height="247" class="alignnone size-full wp-image-1820" /></a>

これで 3D のデータが取り込まれました。

### 編集可能にする

さらにこの時点ではエンティティにロックがかかっているため、編集できません。
  
編集したい場合はエンティティを選択し、&#8221;エンティティ情報&#8221; パネルの &#8220;ロック&#8221; チェックボックスを外します。

<a href="http://blog.1000k.net/wp-content/uploads/su_tutorial_005.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/su_tutorial_005.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/su_tutorial_005.png" alt="su_tutorial_005" width="243" height="176" class="alignnone size-full wp-image-1821" /></a>

これでエンティティが編集可能になります。

<a href="http://blog.1000k.net/wp-content/uploads/su_tutorial_006.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/su_tutorial_006.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/su_tutorial_006-1024x496.png" alt="su_tutorial_006" width="474" height="229" class="alignnone size-large wp-image-1822" /></a>

表面の 2D 画像を消したければ、エンティティを選択して &#8220;サーフェス&#8221; をデフォルトにすれば OK。

## 参考

<a href="https://productforums.google.com/forum/#!topic/sketchup/Crl5SFcyRo0" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://productforums.google.com/forum/#!topic/sketchup/Crl5SFcyRo0', 'How do I import terrain from Google Earth? &#8211; Google プロダクト フォーラム']);" >How do I import terrain from Google Earth? &#8211; Google プロダクト フォーラム</a>