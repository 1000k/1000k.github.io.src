---
title: '[開発環境]NetBeans 6.8 に Zen-Coding を導入する'
author: 1000k
layout: post
date: 2010-05-26
url: /2010/05/26/implement-zen-coding-to-netbeans-68/
categories:
  - その他
  - 開発環境
tags:
  - IDE
  - NetBeans
  - Zen-Coding
  - 開発環境
---
もうZen-Coding無しではHTMLを打てない体になっています。PHPのWEBアプリ開発時にはNetBeansを使うことが多いので、Zen-Codingプラグインを導入してみました。

※ただし、NetBeansではスニペット（単語補完）しか用意されていないようで、liタグのネストなどは使えないようです。

### 導入手順

  1. <a href="http://code.google.com/p/zen-coding/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://code.google.com/p/zen-coding/', 'zen-coding &#8211; Project Hosting on Google Code']);" title="zen-coding - Project Hosting on Google Code">zen-coding &#8211; Project Hosting on Google Code</a> からNetBeans用プラグインをダウンロードする
      * 執筆時のバージョンは「NetBeans.Zen.HTML.1.2.zip」
      * ZIPファイルのまま取り込むので、解凍する必要は無いです
  2. NetBeansのメニューバーから、 **ツール > オプション** を選択
  3. オプションウインドウ左下にある **インポート** をクリック
  4. ダウンロードしたZIPファイルを参照する
  5. 「使用可能なオプション」に **すべて** をチェックして **了解** をクリック。
  6. 「今すぐIDEを再起動」にチェックをつけたままクリック（再起動）

あとはHTMLのソースを開いて、「html:xt」→TABキー などやればスルスルとコードが書けます。

### 少し改良

HTMLへのPHP出力として「＜?php ?＞」ブロックを頻繁に使うので、それもスニペットに追加します。

  1. メニューバー > オプション > エディタ > コードテンプレート > 言語:HTML
  2. 「新規」ボタンをクリック
  3. 下記項目を登録

```
省略名「php」、展開されるテキスト「<?php ${cursor} ?>」
省略名「phpe」、展開されるテキスト「

<?php echo ${cursor}; ?>」
```


以上です。

### 参考

<a href="http://blog.mizoshiri.com/archives/863" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.mizoshiri.com/archives/863', 'Netbeans(windows環境)にZen-Codingを導入']);" title="Netbeans(windows環境)にZen-Codingを導入">Netbeans(windows環境)にZen-Codingを導入</a>