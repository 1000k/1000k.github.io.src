---
title: Adobe Photoshop CCのUIを英語にする方法
author: 1000k
layout: post
date: 2013-09-10
url: /2013/09/10/how-to-change-ui-language-to-eignlish-of-photoshop-cc/
categories:
  - その他
tags:
  - Photoshop
  - トラブルシューティング
---
英語サイトからカスタムアクションをダウンロードして使おうとしたものの、レイヤー名が日本語なせいで、英語版のレイヤー名を前提としたアクションが途中で止まってしまいます。

Photoshop を英語版で動かすようにしたら使えるようになったのですが、そのやり方が直感的ではありませんでした。メモしておきます。

なお、使用バージョンは Adobe Photoshop CC (version 14.0) です。

<!--more-->

まず、Photoshop を開いている場合は終了します。

次に、`{Photoshop のインストールディレクトリ}/Locales/ja_JP/Support Files` の中にある `tw10428.dat` というファイルを、`tw10428.dat.bak` にリネームします。

Windows 7 のデフォルトでは `C:\Program Files\Adobe\Adobe Photoshop CC (64 Bit)\Locales\ja_JP\Support Files` の中にありました。

これで Photoshop を再起動すれば、UI が英語になっています。

高いアドビ税払ってるんだから、このへんのローカリゼーションはちゃんとやってほしいですね。せめて UI の言語ぐらいオプションから設定させてほしいです。（一応 "編集 > 環境設定 > インターフェース"）の中に言語を選ぶセレクトボックスはありますが、日本語しか選べません。じゃあ置くなよ。）

## 参考

[HOW TO CHANGE LANGUAGE IN PHOTOSHOP [Works with (CC, Win, Mac)] - YouTube](http://www.youtube.com/watch?v=3I8B8QH5uRE)