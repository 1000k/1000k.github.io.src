---
title: デコード/エンコードを1ファイルで行うPHP
author: 1000k
layout: post
date: 2011-09-01
url: /2011/09/01/php-script-to-decode-and-encode/
categories:
  - JavaScript
  - jQuery
  - PHP
tags:
  - gist
  - jQuery
  - PHP
  - 自作
---
<a href="http://blog.1000k.net/wp-content/uploads/Encodes-Decodes-localhost-encodeco.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/Encodes-Decodes-localhost-encodeco.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/Encodes-Decodes-localhost-encodeco-278x300.png" alt="" title="エンコード/デコードフォーム" width="278" height="300" class="alignnone size-medium wp-image-805" /></a>

URLエンコード/デコード、HTMLエンティティエンコード/デコードを行うとき便利なフォームを作成しました。
  
PHP+jQueryで実装されており、このファイルをPHPの動くWebサーバーに置けばすぐに使えます。

# できること

  * URLエンコード/デコード
  * HTMLエンティティエンコード/デコード
  * PHPの配列のシリアライズ/アンシリアライズ

# 必要な環境

  * PHPの動作するWebサーバー

<!--more-->

# ソースコード



初めてコードをgistに公開してみました。