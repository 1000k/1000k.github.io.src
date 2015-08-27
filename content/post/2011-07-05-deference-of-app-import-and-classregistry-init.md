---
title: App::import と ClassRegistry::init の違い
author: 1000k
layout: post
date: 2011-07-05
url: /2011/07/05/deference-of-app-import-and-classregistry-init/
categories:
  - CakePHP
  - PHP
tags:
  - CakePHP
  - PHP
---
命名規則に従わないモデルやプラグインをロードする際、使われるのが「App::import」と「ClassRegistry::init」。どういう使い分けをしているのかわからず、使い方によっては期待通り動かなかったりで困っていたので、違いを調べて見ました。

<!--more-->

## App::import

<a href="http://api13.cakephp.org/class/app#method-Appimport" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://api13.cakephp.org/class/app#method-Appimport', 'API Book']);" title="CakePHP: the rapid development php framework: Api : App">API Book</a> によると、下記のような説明があります。

> Finds classes based on $name or specific file(s) to search. Calling App::import() will not construct any classes contained in the files. It will only find and require() the file. 

「コンストラクトは行わない。ファイルを探してrequire()するだけ」と書いています。

また、戻り値は

> boolean true if Class is already in memory or if file is found and loaded, false if not 

となっています。

結局このメソッドは「高機能なrequire」と考えて良さそうです。

## ClassRegistry::init

これも<a href="http://api13.cakephp.org/class/class-registry#method-ClassRegistryinit" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://api13.cakephp.org/class/class-registry#method-ClassRegistryinit', 'API Book']);" title="CakePHP: the rapid development php framework: Api : ClassRegistry">API Book</a>を参考にすると、以下のように書かれています。

> Loads a class, registers the object in the registry and returns instance of the object. ClassRegistry::init() is used as a factory for models, and handle correct injecting of settings, that assist in testing.
    
> Return: object instance of ClassName 

App::importと異なり、「オブジェクトのインスタンスを作成する」と書いています。

なるほど、テストケースの開始時に ClassRegistry::init() を、終了時に ClassRegistry::flush を行なうのも納得がいきます。テスト毎にモデルのインスタンスを初期化→削除しているんですね。

## 使い分け

  * 動的にモデルのインスタンスを使う時は ClassRegistry::init
  * それ以外は App::import

という使い分けで良さそうですです。

また検証していないのですが、「<a href="http://d.hatena.ne.jp/foldrr/20090730/p2" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://d.hatena.ne.jp/foldrr/20090730/p2', 'CakePHP モデルの読み込みは App::import ではなく ClassRegistry::init で &#8211; foldrrの日記']);" title="CakePHP モデルの読み込みは App::import ではなく ClassRegistry::init で - foldrrの日記">CakePHP モデルの読み込みは App::import ではなく ClassRegistry::init で &#8211; foldrrの日記</a>」によると、App::importを使うとDBの接続先が $default 固定になってしまうため、ユニットテストで問題が出るそうです。

## 参考

  * <a href="http://d.hatena.ne.jp/foldrr/20090730/p2" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://d.hatena.ne.jp/foldrr/20090730/p2', 'CakePHP モデルの読み込みは App::import ではなく ClassRegistry::init で &#8211; foldrrの日記']);" title="CakePHP モデルの読み込みは App::import ではなく ClassRegistry::init で - foldrrの日記">CakePHP モデルの読み込みは App::import ではなく ClassRegistry::init で &#8211; foldrrの日記</a>
  * <a href="http://d.hatena.ne.jp/hiromi2424/20101215/1292379625" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://d.hatena.ne.jp/hiromi2424/20101215/1292379625', 'App::import() は凄い &#8211; 24時間CakePHP']);" title="App::import() は凄い - 24時間CakePHP">App::import() は凄い &#8211; 24時間CakePHP</a>
  * <a href="http://web.mt-systems.jp/archives/754" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://web.mt-systems.jp/archives/754', 'CakePHP 今さらですがClassRegistryクラスのメモ | MT Systems']);" title="CakePHP 今さらですがClassRegistryクラスのメモ | MT Systems">CakePHP 今さらですがClassRegistryクラスのメモ | MT Systems</a>
  * <a href="http://d.hatena.ne.jp/bennylee/20091005/1254717512" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://d.hatena.ne.jp/bennylee/20091005/1254717512', 'ClassRegistryの備忘録 &#8211; benny毎日ラボ']);" title="ClassRegistryの備忘録 - benny毎日ラボ">ClassRegistryの備忘録 &#8211; benny毎日ラボ</a>
  * CakeBook API 1.3 
      * <a href="http://api13.cakephp.org/class/class-registry#method-ClassRegistryinit" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://api13.cakephp.org/class/class-registry#method-ClassRegistryinit', 'CakePHP: the rapid development php framework: Api : ClassRegistry']);" title="CakePHP: the rapid development php framework: Api : ClassRegistry">CakePHP: the rapid development php framework: Api : ClassRegistry</a>
      * <a href="http://api13.cakephp.org/class/app#method-Appimport" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://api13.cakephp.org/class/app#method-Appimport', 'CakePHP: the rapid development php framework: Api : App']);" title="CakePHP: the rapid development php framework: Api : App">CakePHP: the rapid development php framework: Api : App</a>