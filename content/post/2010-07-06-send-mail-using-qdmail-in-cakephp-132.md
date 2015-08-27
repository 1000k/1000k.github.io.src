---
title: Qdmailを使ってCakePHP1.3.2のコンソールからメールを送信する
author: 1000k
layout: post
date: 2010-07-06
url: /2010/07/06/send-mail-using-qdmail-in-cakephp-132/
categories:
  - CakePHP
  - PHP
tags:
  - CakePHP
  - Qdmail
  - チュートリアル
---
QdmailはCakePHPを初めとしたフレームワークで簡単にメールを作成・送信できるライブラリです。CakePHP1.2 で利用するサンプルは見つかったのですが、1.3で使っている例をあまり見かけず、少しつまづいたのでメモしておきます。

### 手順

1&#46; <a href="http://hal456.net/qdmail/downloads" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://hal456.net/qdmail/downloads', 'Qdmailをダウンロード']);" title="ダウンロードdownload - Qdmail - PHP::Mail Library , Quick and Detailed for Multibyte">Qdmailをダウンロード</a>し、/app/controllers/components に解凍する

2&#46; コンソールプログラムを下記のように作成する

```
// /app/vendors/shells/uso.php

<?php
class UsoShell extends Shell {
    // 使用するコア
    var $Controller;

    // 使用するコンポーネント
    var $Qdmail;

    // overrideしてcake起動メッセージを消す
    function startup() {}

    // 初期化処理
    function initialize() {
        parent::initialize();

        App::import("Core", "Controller");
        App::import("Component", "Qdmail");

        $this->Controller =&#038; new Controller();
        $this->Qdmail =&#038; new QdmailComponent(null);
        $this->Qdmail->startup($this->Controller);
    }

    // エントリーポイント
    function main() {

        // Qdmailの設定
        $this->Qdmail->to('to@uso.uso', '送信先の名前');
        $this->Qdmail->subject('件名');
        $this->Qdmail->from('from@uso.uso' , '送信元の名前');
        $this->Qdmail->text('本文');

        // 送信する
        $return = $this->Qdmail->send();
        $this->Qdmail->reset();

        if ($return) {
            $this->out("送信完了。");
        } else {
            $this->out("送信失敗...");
        }
    }
}
?>
```


### 補足

何が悪いのかよくわかりませんが、上のコードを実行すると「指定されたパスが見つかりません。」と表示されます。送信するぶんには問題ないのですが、気持ち悪いのでどうにか消せないか方法を探しています。

### 参考

  * <a href="http://hal456.net/qdmail/cakebase" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://hal456.net/qdmail/cakebase', '使い方　CakePHPでのメール送信 &#8211; Qdmail &#8211; PHP::Mail Library , Quick and Detailed for Multibyte']);" title="使い方　CakePHPでのメール送信 - Qdmail - PHP::Mail Library , Quick and Detailed for Multibyte">使い方　CakePHPでのメール送信 &#8211; Qdmail &#8211; PHP::Mail Library , Quick and Detailed for Multibyte</a>
  * <a href="http://c-brains.jp/blog/wsg/10/06/10-110652.php" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://c-brains.jp/blog/wsg/10/06/10-110652.php', '[ステップアップ！ CakePHP] Shell を使ってコマンドラインで CakePHP | バシャログ。']);" title="[ステップアップ！ CakePHP] Shell を使ってコマンドラインで CakePHP | バシャログ。">[ステップアップ！ CakePHP] Shell を使ってコマンドラインで CakePHP | バシャログ。</a>
  * <a href="http://bakery.cakephp.org/articles/view/emailcomponent-in-a-cake-shell" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://bakery.cakephp.org/articles/view/emailcomponent-in-a-cake-shell', 'EmailComponent in a (cake) Shell (Articles) | The Bakery, Everything CakePHP']);" title="EmailComponent in a (cake) Shell (Articles) | The Bakery, Everything CakePHP">EmailComponent in a (cake) Shell (Articles) | The Bakery, Everything CakePHP</a>
  * <a href="http://c-brains.jp/blog/wsg/10/06/10-110652.php" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://c-brains.jp/blog/wsg/10/06/10-110652.php', '[ステップアップ！ CakePHP] Shell を使ってコマンドラインで CakePHP | バシャログ。']);" title="[ステップアップ！ CakePHP] Shell を使ってコマンドラインで CakePHP | バシャログ。">[ステップアップ！ CakePHP] Shell を使ってコマンドラインで CakePHP | バシャログ。</a>