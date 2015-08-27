---
title: スタブとモック
author: 1000k
layout: post
date: 2011-04-07
url: /2011/04/07/what-is-stub-and-mock/
categories:
  - SimpleTest
  - テスト
tags:
  - test
  - UnitTest
---
今までモックをほとんど使ったことがなかったので、勉強してみました。

## スタブとは

ref: <a href="http://ja.wikipedia.org/wiki/%E3%82%B9%E3%82%BF%E3%83%96" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://ja.wikipedia.org/wiki/%E3%82%B9%E3%82%BF%E3%83%96', 'スタブ &#8211; Wikipedia']);" title="スタブ - Wikipedia">スタブ &#8211; Wikipedia</a>

> 呼び出す側（上位）のモジュールを検査する場合に、呼び出される側（下位）の部品モジュールが未完成であることがある。このとき、呼び出される側の部品モジュールの代用とする仮のモジュールを、「スタブ」と呼ぶ。スタブモジュールは設計仕様に定義されている全ての関数を実装してあるが、関数内部は正規の動作をする事無く適当な定数を返すというような作りになっている事が多い。 

「必ずfalseを返すスタブ」「ランダムな整数を返すスタブ」なんて言葉はよく聞きます。

## モックとは

ref: <a href="http://d.hatena.ne.jp/sekom/20090702/p1" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://d.hatena.ne.jp/sekom/20090702/p1', 'スタブとモックの違い &#8211; ソフト開発お仕事メモ']);" title="スタブとモックの違い - ソフト開発お仕事メモ">スタブとモックの違い &#8211; ソフト開発お仕事メモ</a>

モック戦略を使用する際には、3つの手順を踏むことになります。

| No | 手順名                  | 説明                                                                           |
| -- | -------------------- | ---------------------------------------------------------------------------- |
| 1  | 期待値の設定               | モックオブジェクトに対して、メソッドが呼び出されるべき順序を記録します。その際に、期待される引数、モックオブジェクトのメソッドが返す戻り値も設定します。 |
| 2  | テスト条件下でのモックオブジェクトの使用 | モックオブジェクトを使用したテストを実施します。                                                     |
| 3  | 結果の検証                | モックオブジェクトに対し、期待されたとおりにモックオブジェクトが使用されたか問い合わせます。                               |

## スタブやモックが必要になる理由

ref: <a href="http://www39.atwiki.jp/startruby/pages/23.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www39.atwiki.jp/startruby/pages/23.html', 'Start! Ruby &#8211; RSpecの構文']);" title="Start! Ruby - RSpecの構文">Start! Ruby &#8211; RSpecの構文</a>

  * 全てを「本物」でテストしようとすると、「全てが揃わないとテストできない」という本末転倒な事が起こりかねない。
  * たとえば時刻に関するオブジェクトのように、システムの構成によって変化してしまうオブジェクトがあると、テスト環境によって差異ができてしまう。
  * UnitTestが大きな問題に移ると段々と結合テスト化してしまう、という問題がある。

※ ただし、スタブ/モックを多用し過ぎると、今度はインタフェース不一致の発見を先送りにする、という状況にもなりかねない。このあたりはさじ加減が必要。

## モックでできること

ref: <a href="http://gihyo.jp/dev/feature/01/php-test/0004" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://gihyo.jp/dev/feature/01/php-test/0004', 'PHPUnit3で始めるユニットテスト：第4回　モックオブジェクトを使ったテスト｜gihyo.jp … 技術評論社']);" title="PHPUnit3で始めるユニットテスト：第4回　モックオブジェクトを使ったテスト｜gihyo.jp … 技術評論社">PHPUnit3で始めるユニットテスト：第4回　モックオブジェクトを使ったテスト｜gihyo.jp … 技術評論社</a>

  * 生成されるオブジェクトにメソッドを定義する
  * そのメソッドの振る舞いを指定する 
      * 実行回数の制約を設ける 
          * たとえば「1回のみ呼び出される」や「0回以上呼び出される」
      * メソッド名を指定する
      * 具体的な振る舞いを記述する 
          * メソッドの戻り値
          * メソッドが投げる例外

## スタブとモックの違い

ref: <a href="http://www.ibm.com/developerworks/jp/web/library/wa-mockrails/index.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.ibm.com/developerworks/jp/web/library/wa-mockrails/index.html', 'Ruby on Rails でのモックとスタブの作成']);" title="Ruby on Rails でのモックとスタブの作成">Ruby on Rails でのモックとスタブの作成</a>

> 「<a href="http://d.hatena.ne.jp/devbankh/20100210" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://d.hatena.ne.jp/devbankh/20100210', 'モックとスタブの違い &#8211; [lib]']);" title="モックとスタブの違い - [lib]">モックとスタブの違い &#8211; [lib]</a>」
> 
> モック・オブジェクトは一種のスタブです。モック・オブジェクトは、テスト対象のオブジェクトを使用するクライアント・コードを置き換えます。しかしモック・オブジェクトはそれ以上のことを行い、テスト対象のオブジェクトがクライアント・コードを実際にどう使うかを測定するのです。
> 
> インターフェースの使い方をテストする場合にはモックを、インターフェースの使い方をまったく気にしない場合にはスタブを使う必要があります。
> 
> モック・オブジェクトの作成は、スタブの作成とよく似ています。違いは、スタブは受動的であるということです。スタブは、スタブの作成対象のメソッドに対して呼び出しを行う実在のソリューションを単にシミュレーションするにすぎません。一方モックは能動的であり、モック・オブジェクトを使って行うその方法を実際にテストします。想定の動作と一致する方法でモックを使わないと、テストは失敗します。 

ref: <a href="http://capsctrl.que.jp/kdmsnr/wiki/bliki/?TestDouble" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://capsctrl.que.jp/kdmsnr/wiki/bliki/?TestDouble', 'Martin Fowler&#8217;s Bliki in Japanese &#8211; テストダブル']);" title="Martin Fowler's Bliki in Japanese - テストダブル">Martin Fowler&#8217;s Bliki in Japanese &#8211; テストダブル</a>

> スタブは、テスト時の呼び出しに対して、あらかじめ用意された結果を返す。通常、テスト用にプログラムされたところ以外には応答しない。スタブは呼び出しの情報を記録することもある。例えば、Eメールゲートウェイスタブは「送られた」メッセージを記録するような場合だ。単に「送られた」メールの数を記録する場合もあるだろう。
> 
> モックは、エクスペクテーションが事前にプログラムされたものである。エクスペクテーションとは、受信する一連の呼び出しの仕様を表わしたものである。期待されない呼び出しが行なわれた場合は例外をスローする。また、テスト実行後の検証(verification)で、期待された呼び出しがすべてきちんと行われたかどうかを確認する。