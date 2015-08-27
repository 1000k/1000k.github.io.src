---
title: '[CodeIgniter] 覚えておくべきルール（命名規則、禁則）'
author: 1000k
layout: post
date: 2010-05-24
url: /2010/05/24/coding-rules-of-codeigniter/
categories:
  - CodeIgniter
  - PHP
tags:
  - CodeIgniter
  - PHP
---
CodeIgniterのルールの中には、「じゃあそんなオプション消しとけよ」と思うようなこともあるので、メモしておきます。

すべての根拠は <a href="http://codeigniter.jp/user_guide_ja/general/styleguide.html#class_and_method_naming" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://codeigniter.jp/user_guide_ja/general/styleguide.html#class_and_method_naming', 'PHPコーディングスタイル : CodeIgniter ユーザガイド 日本語版']);" title="PHPコーディングスタイル : CodeIgniter ユーザガイド 日本語版">PHPコーディングスタイル : CodeIgniter ユーザガイド 日本語版</a> から。

## ★short\_open\_tag は使わない

サーバーで short\_open\_tag が有効でないケースもあるので、常に完全なPHP開始タグを用います。

## インデントにはスペースではなくタブを使う

言語やフレームワークによってこのあたりはまちまちですが、CIではタブ推奨。以下がその理由。

  * スペースの代わりにタブを使う事で、開発者がどのようなアプリケーションを利用していても、各自が見やすくカスタマイズしたインデントレベルでコードを見る事が可能になる
  * さらに副次的な効果として、例えば4つのスペースに対して1つのタブを用いる事で(わずかではありますが)ファイルがよりコンパクトになる

（個人的には、半角スペースでインデントされたソースは、キーボードで移動するとき連打を強要させられるので、このルールには賛成です。）

## TRUE, FALSE, NULL は、常に大文字を使う

理由はよくわからないけど、そうするらしいです。

## PHPの終了タグは書かない

「?>」はエラーに繋がるので書かない。

## ★命名規則

キャメルケースは使わず、アンダースコアとの組み合わせで表現する。

### クラス名: 先頭大文字＋アンダースコア

```
間違い:
class superclass
class SuperClass

正しい:
class Super_class
```


### ファイル名、変数名: 小文字＋アンダースコア

```
間違い:
function fileproperties()   // 表現がわかりにくく、アンダースコアが抜けている
function fileProperties()   // 表現がわかりにくく、キャメルケースが使われている
function getfileproperties()    // ベター! しかしながらアンダースコアが抜けている
function getFileProperties()    // キャメルケースが使われている
function get_the_file_properties_from_the_file() // 長過ぎる

正しい:
function get_file_properties()  // 説明的、アンダースコア、全て小文字
```
