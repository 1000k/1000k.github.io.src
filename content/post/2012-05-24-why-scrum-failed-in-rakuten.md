---
title: Scrum失敗例 楽天の場合
author: 1000k
layout: post
date: 2012-05-24
url: /2012/05/24/why-scrum-failed-in-rakuten/
categories:
  - Scrum
  - アジャイル
tags:
  - Agile
  - Scrum
  - レビュー
---
楽天のあるプロジェクトが「Scrumをやったもののうまくいかなかった」という経験談をスライドにまとめていました。
  
概要と感想を書きます。

<div style="width:425px" id="__ss_13014872">
  <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/TakaoOyobe/20120521-13014872" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.slideshare.net/TakaoOyobe/20120521-13014872', '私がスクラムをやめた理由 &#8211; 全員スクラムマスター。＠DevLove &#8211;']);" title="私がスクラムをやめた理由 - 全員スクラムマスター。＠DevLove -" target="_blank">私がスクラムをやめた理由 &#8211; 全員スクラムマスター。＠DevLove &#8211;</a></strong> 
  
  <div style="padding:5px 0 12px">
    View more <a href="http://www.slideshare.net/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.slideshare.net/', 'presentations']);" target="_blank">presentations</a> from <a href="http://www.slideshare.net/TakaoOyobe" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.slideshare.net/TakaoOyobe', 'Takao Oyobe']);" target="_blank">Takao Oyobe</a>
  </div></p>
</div>

<!--more-->

## スライド概要

Scrumを導入したが、下記の問題により挫折。

  * 人が選べない
  * プラクティスに従ってくれない人がいる
  * 切符を切る人（正しくバスに乗っているか確かめる人）がいない
  * 役割は決められない
  * 結局新たな滝（ウォーターフォール）ができただけ
  * 教育が難しい 

## スライドを見て思ったこと

  * 根本的にスクラムマスターの権限が弱すぎる？ 
      * プラクティスを周知し、従わせる
      * 合わない場合はプラクティスを軽量化するか捨てる
      * どうしても従わないメンバーがいる場合、入れ替えられる機構が必要
  * 結局スモールウォーターフォールになるのは自分も経験済み。恐らく「スモールリリース」に誤解 
      * 1スプリント毎に必ず成果をコミットする（タイムボックス化）
      * 優先度の高いストーリーから消化する
  * メンバーをバスに乗せるのは本当に難しい 
      * 「アジャイル開発のプラクティスをやれば良くなる」という自発的な動機がなければ、大量のプラクティスの学習で脱落する人が続出しそう
      * スクラムマスターはここでも継続的・漸進的な推進をする必要がありそう
      * 最初の2～4スプリントはふつうスパイクや調査の期間なので、ここで徐々に教育すれば良さそう

総じて、メンバーをプラクティスに従わせるのに苦労している印象を受けました。

アジャイル開発には自律的なプラクティスが多く、自ら行ってもらえるようにならないとうまく回りません。
  
メリットが実感できればもっと積極的になってくれるかもしれませんが。

<a href="http://www.ryuzee.com/contents/blog/?s=%E3%83%AF%E3%83%BC%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%83%E3%83%97" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.ryuzee.com/contents/blog/?s=%E3%83%AF%E3%83%BC%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%83%E3%83%97', 'プラクティスの意義を実感するためのワークショップ']);" title="ワークショップ | Ryuzee.com">プラクティスの意義を実感するためのワークショップ</a>（短時間でできるゲーム）が多数あるので、これをまず体験させるのも効果的でしょう。

あとこれは私の経験則ですが、開発メンバーの主力がスクラムマスターを兼任すると、開発も指揮も中途半端になるのでオススメしません。