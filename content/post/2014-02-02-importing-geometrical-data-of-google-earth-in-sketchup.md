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

"ファイル > ジオロケーション > 場所を追加" をクリック。

{{< img src="/img/su_tutorial_001.png" title="su_tutorial_001" >}}

Google Maps が開くので、地名を検索する。

"地域を選択" をクリック。

矩形の範囲を設定し、"グラブ" をクリック。

{{< img src="/img/su_tutorial_002-300x224.png" title="su_tutorial_002" >}}

これで地形データが Sketchup にインポートされます。

{{< img src="/img/su_tutorial_003-1024x680.jpg" title="su_tutorial_003" >}}

### 3D モデルを表示する

この時点では 2D の画像しかありませんが、3D のデータも非表示のレイヤーで取り込まれています。

表示するには次の手順に従います。

  1. "ウィンドウ > レイヤ" をクリックして、レイヤパネルを開く。
  2. "Google Earth Terrain" の "可視" チェックボックスをクリック。

{{< img src="/img/su_tutorial_004.png" title="su_tutorial_004" >}}

これで 3D のデータが取り込まれました。

### 編集可能にする

さらにこの時点ではエンティティにロックがかかっているため、編集できません。

編集したい場合はエンティティを選択し、"エンティティ情報" パネルの "ロック" チェックボックスを外します。

{{< img src="/img/su_tutorial_005.png" title="su_tutorial_005" >}}

これでエンティティが編集可能になります。

{{< img src="/img/su_tutorial_006-1024x496.png" title="su_tutorial_006" >}}

表面の 2D 画像を消したければ、エンティティを選択して "サーフェス" をデフォルトにすれば OK。

## 参考

[How do I import terrain from Google Earth? - Google プロダクト フォーラム](https://productforums.google.com/forum/#!topic/sketchup/Crl5SFcyRo0)