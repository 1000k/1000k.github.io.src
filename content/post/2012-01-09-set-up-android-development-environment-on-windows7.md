---
title: Windows 7上にAndroid開発環境を構築する
author: 1000k
layout: post
date: 2012-01-09
url: /2012/01/09/set-up-android-development-environment-on-windows7/
categories:
  - Android
  - その他
  - 開発環境
tags:
  - Android
  - Eclipse
  - Java
  - SDK
---
「Androidは簡単に開発が可能ですよ！」と言われてる割に導入がかなり面倒でした。

必要なパッケージとインストールからエミュレーターの起動まで、やり方をメモしておきます。

<!--more-->

## 手順

### 1. 必要ファイルのダウンロード

Windows 7 64bitの場合、下記アプリをダウンロードしました。

  * Eclipse 3.7 (Indigo) Java Developer Edition
      * [Eclipse Downloads](http://www.eclipse.org/downloads/)
      * Eclips IDE for Java Developers
  * Java 6 JDK
      * [Java SE Downloads](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
      * JREではなくJDK！
      * 執筆時バージョンは Java SE 6 Update 30
  * Android SDK
      * [Android SDK | Android Developers](http://developer.android.com/sdk/index.html)
      * installer_r16-windows.exe (Recommended)

### 2. Java 6 JDKの設定

インストールしたファイルを実行して、画面に従うだけです。

### 3. Android SDKの設定

  1. ダウンロードしたファイルを解凍orインストーラー実行
  2. Android SDKのインストールが完了後、Managerが起動するので、使いたいAPIにチェックして[Install * Packages]
      * 2.1を選べば97%のAndroidユーザーをカバーできるらしいので、チェック
      * 必要に応じて別のバージョンもインストールする

### 4. Eclipseの設定

#### Android Developer Tools (ADT)のインストール

  1. ダウンロードしたファイルを解凍し、eclipse.exeを実行
  2. Eclipseを起動し、[Help] - [Install software]
  3. [Work with:]の隣にある[Add]をクリック。完了したら[OK]
      * Name: Android Plugin
      * Location: http://dl-ssl.google.com/android/eclipse/
      * 「https」を指定すると途中でエラーが出て止まる
  4. [Work with:]に今入力した項目を選択。数十秒「Pending…」が表示された後、[Developer Tools]が出てきたらチェックを付けて[Next]
  5. インストールされるパッケージ一覧が出るので、[Finish]
  6. 途中「Security Warning」が出るが、[OK]
  7. 長時間インストール完了を待ち、完了したら[Restart Now]
  8. Eclipse再起動後、「Welcome to Android Development」が表示されるので、[Use existing SDKs]にチェック
  9. SDKのインストール先を指定して[Next]
      * デフォルトなら "C:\Program Files (x86)\Android\android-sdk"
 10. 統計データをGoogleに送るかどうか選択して[Finish]

#### AVDの作成

  1. [Window] - [AVD Manager]
  2. [New]
  3. 下記項目を入力して[Create AVD]
      * [Name]: 機器名。好きな名前でOK
      * [Target]: Androidのバージョン
      * [SD Card]: エミュレータが使用するSDカード容量。32MBもあれば十分
      * [Skin]: 画面解像度
      * [Hardware]: エミュレータのハード。GPSなど、開発しようとするアプリに合わせて選択
  4. [Start]をクリックして、エミュレータが起動したらOK
      * 日本語化したい場合
      * 起動後にホーム画面でメニューボタンクリック
      * [Settings] > [Language & keyboard]
      * [Select locale］から日本語を選択

設定は以上で完了です。

### 5. サンプルアプリのビルド

ここまででビルド可能になっているので、試してみましょう。

  1. [File] - [New] - [Other…]
  2. Android Project を選択して[Next]
  3. 以下のように設定して[Next]
      * Project Name: SampleApp
      * "Create projects from existing source"にチェック
      * Location: "C:\Program Files (x86)\Android\android-sdk\samples\android-7\SkeletonApp"
  4. Build Targetは"Android 2.1&#8243;にチェックして[Next]
  5. Minumum SDKを"7&#8243;にして[Finish]
  6. [Run] - [Run Configurations…]
  7. [New launch configuration]アイコンをクリック
  8. [Project:]に"SampleApp"を指定して[Run]
  9. エミュレーター上に"Hello there, you Activity!"と表示されれば成功

[{{< img src="/img/android_tutorial-300x293.png" title="" >}}](http://blog.1000k.net/wp-content/uploads/android_tutorial.png)

なお、ビルドされたアプリはbinディレクトリの下に「SampleApp.apk」という名前で入っています。これをAndroidに転送すれば実機で実行できます。

（ただし「提供元不明のアプリ」を使用可能にしておくこと）

## 参考

  * [世界を目指せ！Androidアプリ開発入門：第2回　Androidアプリ開発のための環境構築｜gihyo.jp … 技術評論社](http://gihyo.jp/dev/serial/01/androidapp/0002)
      * 2010年の記事なので少し古い部分もありますが、基本的に最新バージョンで読み替えればOK
  * [世界を目指せ！Androidアプリ開発入門：第3回　Android SDKでサンプルアプリを使ってみる｜gihyo.jp … 技術評論社](http://gihyo.jp/dev/serial/01/androidapp/0003)
  * [Android開発環境の構築 Eclipse編](http://sky.geocities.jp/izeefss/develop/android/env_eclipse.html)