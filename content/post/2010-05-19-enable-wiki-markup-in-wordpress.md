---
title: '[WordPress] WordPressでwiki記法を有効にする'
author: 1000k
layout: post
date: 2010-05-19
url: /2010/05/19/enable-wiki-markup-in-wordpress/
categories:
  - WordPress
tags:
  - WordPress
  - プラグイン
---
# きっかけ

WordPressデフォルトのテキストエディタは以下の理由で使いづらいです。

  * ビジュアルエディタは重い＆ときどき妙なブロックを作る
  * シンプルエディタは入力が面倒
  * 特に見出しやリストを気軽に作れないのが面倒

wiki記法なら多少は楽になるだろうと思い、「WP MarkItUp! WordPress plugin」プラグインを導入してみました。

# 導入環境

  * WordPress 2.9.2

# 手順

WordPress内のプラグインインストーラーだけで完結します。

  1. WordPress管理画面にて、 [プラグイン]->画面の上の方にある[新規追加] をクリック
  2. 「プラグインのインストール」画面の検索窓で、「MarkItUp」で検索
  3. 検索結果に「WP MarkItUp!」が出てくるので、テーブル一番右の「インストール」をクリック
  4. 同様の手順で、「MarkDown」で検索→「Markdown for WordPress and bbPress」プラグインをインストール
      * ※今回はMarkDown記法を使いたいので、このプラグインも必要になります
  5. 両方のプラグインを有効化したら、 管理画面で[設定]->[WP MarkItUp!] を開く
  6. 「Tag set to use:」は「MarkDown」を、「Skin:」と「Editor Default Height:」は好きなやつを選んで [Save Changes]ボタンをクリック

あとはいつもの編集画面がMarkDown用に変わっているので、好きに書けばいいでしょう。

初めてMarkDown記法を使ってみましたが、悪くないです。記法の詳細は <a href="http://bono.s206.xrea.com/2007/01/312-markdown_syntax/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://bono.s206.xrea.com/2007/01/312-markdown_syntax/', 'power source* » WP: PHP Markdown 記法早見表（的なもの）']);" title="power source* » WP: PHP Markdown 記法早見表（的なもの）">power source* » WP: PHP Markdown 記法早見表（的なもの）</a> あたりを見れば十分です。

# トラブルシューティング

## 「Syntax Highlighter」と競合する場合

このブログでは「Syntax Highlighter for WordPress」を入れていますが、&#91;code]～[/code]ブロック内までMarkDown記法と解釈され、おかしな表示になることがあります。

そんなときは、&#91;code]～[/code] をdivブロックで囲ってやると、MarkDownが無視して正常に表示されます。

```
```
 ... ```



<p>
  参考: <a href="http://wordpress.org/support/topic/293458" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://wordpress.org/support/topic/293458', 'WordPress › Support » [Plugin: SyntaxHighlighter Evolved] Conflict with Markdown Extra plugin']);" title="WordPress › Support » [Plugin: SyntaxHighlighter Evolved] Conflict with Markdown Extra plugin">WordPress › Support » [Plugin: SyntaxHighlighter Evolved] Conflict with Markdown Extra plugin</a>
</p>