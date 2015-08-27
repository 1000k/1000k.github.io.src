---
title: 階段の作り方いろいろ
author: 1000k
layout: post
date: 2010-12-13
url: /2010/12/13/how-to-draw-steps/
categories:
  - SketchUp
tags:
  - SketchUp
  - TIPS
---
SketchUp で階段を作るのは結構面倒くさいです。以下にいくつかのやり方をメモします。

<!--more-->

## The Subdivided Rectangles method



  1. ベースとなる長方形を作る
  2. 長方形を1ステップぶん刻み、直線をコピーして段数ぶんの切り込みを作る
  3. 鉛直方向に直線を引き、1段ぶんの高さで刻む
  4. [プッシュ/プルツール]で**高い方から順に**ガイドを使って盛り上げる 
      * 低い方から作ると、1段ごとに「オフセットの限界」と言われてしまう

## The Copied Profile method



  1. 直方体を作る
  2. 側面に「┌」という傾けたL字を作る
  3. 傾けたL字をコピーして、1個目の右上の点が2個目の左下の点になるようにする
  4. 「*n」(nは整数)を入力すれば、n段の階段ができる
  5. [プッシュ/プルツール]で側面から反対面まで押して削ってやれば階段ができる 
      * 2.で作る1段目を工夫すれば、切り込みのある階段なども作成可能

## The Treads are Components method



  1. 1ステップぶんの直方体を作り、コンポーネント化する
  2. 2段目の左下角が1段目の右上角に当たるようにコピーする
  3. 「*n」(nは整数)を入力すれば、n段の階段ができる
  4. コンポーネント編集に移り、薄さを変えたり、手すりを付けたりする 
      * 全段が同時に編集できるので追跡しやすく便利