---
title: Selenium IDE + Pearl Crescent Page Saver で複数画面のキャプチャを自動化する
author: 1000k
layout: post
date: 2010-06-11
url: /2010/06/11/automate-screen-capture-using-selenium-ide-pearl-crescent-page-saver/
categories:
  - FireFoxアドオン
  - Selenium
  - テスト
tags:
  - Selenium
  - test
  - チュートリアル
  - 自動化
---
多くの画面があるサイトで、すべてのページを、スクロール領域すべてを含めてキャプチャしなければならない。

こんなこと手動でやっていては大変です。しかも画面修正が入って「もう一回全部撮り直せ」と言われたらもう死ぬしかないですね。

早まる前に、この記事のやり方で自動化してください。

<!--more-->

## 必要なもの

  * FireFox 3.x
  * [Selenium IDE](http://seleniumhq.org/download/) (執筆時 バージョン1.0.7)
  * [Pearl Crescent Page Saver](http://pearlcrescent.com/products/pagesaver/)

## 手順

まず、Pearl Crescent Page Saver のショートカット設定をします。「Page Saverオプション」から、下記の通り設定します。

  * 「全般」タブ
      * 「キーボードショートカットかツールバーのボタンがクリックされた時:」 - **ページ全体**
      * 「キーボードショートカット:」 - **Alt+w** (好きなキーで構わないですが、下記はこの設定でやります)
  * 「画像のキャプチャー」タブ
      * 「ファイルの保存名:」 - **%t %u**
      * 「次のフォルダに保存する」 - 好きなフォルダ
      * 「同じ名前のファイルがあるときは上書きする」 - **チェックする**

次に、Selenium IDEのテストケースを作成します。Selenium IDEの基本的な使い方は 「[ブラウザを選ばずWebテストを自動化するSelenium (2/3) - ＠IT](http://www.atmarkit.co.jp/fjava/rensai4/devtool07/devtool07_2.html)」 が詳しいです。

<table>
  <tr>
    <th>
      コマンド
    </th>

    <th>
      対象
    </th>

    <th>
      値
    </th>
  </tr>

  <tr>
    <td>
      open
    </td>

    <td>
      /index.php
    </td>

    <td>
    </td>
  </tr>

  <tr>
    <td>
      windowFocus
    </td>

    <td>
    </td>

    <td>
    </td>
  </tr>

  <tr>
    <td>
      altKeyDown
    </td>

    <td>
    </td>

    <td>
    </td>
  </tr>

  <tr>
    <td>
      keyPress
    </td>

    <td>
      //
    </td>

    <td>
      \119
    </td>
  </tr>

  <tr>
    <td>
      altKeyUp
    </td>

    <td>
    </td>

    <td>
    </td>
  </tr>
</table>

これが1ページぶんのオープン～キャプチャの流れです。

ここまで設定したら、Selenium IDEの「現在のテストケースを実行」をクリックしてください。指定したURLが開き、先ほど指定したフォルダに画像が出力されていればOKです。

あとは上記の要領で、「open」コマンドの対象を撮影したいページのアドレスに変更しながらコピペを繰り返してください。

## 日本語を含むテストケースファイルが読み込めない場合

  * XHTMLのヘッダを「xml:lang="en" lang="en"」から「xml:lang="ja" lang="ja"」に直す
  * ファイル保存形式を「UTF-8 BOM無し」にする

## TIPS

カテゴリ毎に多くのコンテンツがあるようなページを連続キャプチャする場合、カテゴリ毎にテストケースを作成し、それをテストスイートに読み込ませると管理が便利（だと思います）。

## 参考

[Seleniumでキャプチャを取得する拡張コマンド:captureScreenshot - 現場のためのソフトウェア開発プロセス - たかのり日記](http://d.hatena.ne.jp/szk-takanori/20071104/1194181489)