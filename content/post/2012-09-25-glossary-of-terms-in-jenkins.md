---
title: Jenkinsを触ると出てくる用語の解説
author: 1000k
layout: post
date: 2012-09-25
url: /2012/09/25/glossary-of-terms-in-jenkins/
categories:
  - 開発環境
tags:
  - Jenkins
---
Jenkinsは元々Javaのビルドを自動化するツールだったため、Javaの知識が無いと理解しづらい部分があります。

以下、必要になる知識を簡単にまとめておきます。

<!--more-->

## ビルド

Java は PHP や ruby と異なり、ソースのままでは実行できません。実行前に必ず_コンパイル_を行い、実行可能形式（JARファイル）に変換する必要があります。

実行可能形式を作成するための一連の流れを_ビルド_と呼びます。ビルドでは主に以下の作業を行います。

  * ソースの文法解析
  * テストの実行
  * ドキュメントの作成（Javadocなど）
  * コンパイル (実行可能ファイルの作成)

これらすべてのコマンドを手動で打つのは困難です。そのため、自動でビルドを行うビルドツールが存在します。代表的なものとして make, Ant, Maven などがあります。（後述）

PHP等のスクリプト言語ではコンパイルは不要ですが、コンパイル以外のプロセスはJavaと共通のため、「ソースが実行可能な状態にする」という意味でビルドという用語をそのまま使っているようです。

参考:

  * [ビルドは何をしている？ [VC++の使い方]](http://www.nitoyon.com/vc/tutorial/project/build_detail.htm)
  * [ビルド (ソフトウェア) - Wikipedia](http://ja.wikipedia.org/wiki/%E3%83%93%E3%83%AB%E3%83%89_(%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2))

## デプロイ

作成したコードを利用可能にすること。「本番反映」とも言い換えられます。

Java ではビルドで作成したJARファイルやWARファイルをWebサーバー（Apache Tomcatなど）に読み込ませ、再起動することで利用可能になります。

PHP や Ruby などのスクリプト言語なら、動作中のWebサーバーにソースコードを配置するだけです。

## Ant

  * ビルドツール。
  * [GNU make](http://ja.wikipedia.org/wiki/Make) の代替。
  * ビルドファイル（build.xml）にビルドルールを書くことで、ビルドを自動化できます。

参考:

  * [Apache Ant - Wikipedia](http://ja.wikipedia.org/wiki/Apache_Ant)

## Maven

  * プロジェクト管理ツール。
  * Apache Ant の機能を内包しており、単にビルドツールとしても利用可能です。
  * コンパイル、テスト、Javadoc生成、テストレポート生成、デプロイなど、様々な機能が用意されています。
  * Ant では build.xml に細かい指示を記述する必要があったが、Maven では指示をコマンドラインに記述するだけで良い。
  * Jenkins は Ant/Maven どちらも利用可能です。

参考:

  * [2. Maven 入門 | TECHSCORE(テックスコア)](http://www.techscore.com/tech/Java/ApacheJakarta/Maven/2/)
  * [Apache Maven - Wikipedia](http://ja.wikipedia.org/wiki/Apache_Maven)
  * [Apache MavenとAntの違いについて調べてみた - Shuichi’Tec](http://d.hatena.ne.jp/nsas454/20101013/1287328377)