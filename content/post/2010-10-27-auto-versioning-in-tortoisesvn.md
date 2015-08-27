---
title: TortoiseSVNでバージョン情報などを自動で埋め込む
author: 1000k
layout: post
date: 2010-10-27
url: /2010/10/27/auto-versioning-in-tortoisesvn/
categories:
  - PHP
  - その他
tags:
  - Subversion
  - TortoiseSVN
  - トラブルシューティング
---
オープンソースのコードを見ていると、リビジョン番号などがソースに記載してあるケースがよくあります。

どうやら Subversion（や CVS）で実現できるようです。

私は基本的に Windows で TortoiseSVN を使ってリポジトリのやりとりをしているので、その設定方法をメモしておきます。

<!--more-->

## 手順

### 1.TortoiseSVNの設定

今回は例としてPHPファイルが自動変換されるよう設定します。

  1. svn管理下のファイルを右クリック->TortoiseSVN->設定
  2. [一般]カテゴリ内の、「Subversionの設定ファイル」の横にある[編集]ボタンをクリック
  3. テキストエディタでconfigファイルが開くので、以下のように修正する

```
enable-auto-props = yes    ←コメントアウトを外す

[auto-props]
*.php = svn:keywords=Id Date Author Rev
```


### 2.対象ファイルにsvnキーワードを埋め込む

キーワードは「**$Id$**」のように、「$」で囲みます。

使用可能なキーワードは [Subversion キーワードの展開 - とみぞーノート](http://wiki.bit-hive.com/tomizoo/pg/Subversion%20%A5%AD%A1%BC%A5%EF%A1%BC%A5%C9%A4%CE%C5%B8%B3%AB) あたりを参考にしてください。

```
/**
 * テストクラス
 *
 * @author  $Author$
 * @version $Id$
 */
class Uso
{
    ...
}
```


### 3.コミットしてみる

ファイルをコミットしてみて、リポジトリブラウザで参照して置換されていれば成功です。

```
/**
 * テストクラス
 *
 * @author  $Author: Senda Keijiro $
 * @version $Id: uso.php 13 2010-10-27 05:33:51Z Senda Keijiro $
 */
class Uso
{
    ...
}
```


## 注意

**自動置換は上記設定を行った以後に作られたファイルのみに行われます。**

既存のファイルも自動反映したい場合は、ファイル毎に設定する必要があります。

  1. 既存のファイルを選択して右クリック
  2. [Subversion]タブ->[属性]ボタン
  3. 恐らく未設定なので、[新規]ボタンをクリック
  4. [属性名]に「svn:keywords」、[属性値]に「Id Author Rev Date」(1単語ずつ改行)を書いて[OK]

これで、ファイルをコミットすると自動でキーワードが置換されるようになります。

## 参考

  * [Subversion キーワードの展開 - とみぞーノート](http://wiki.bit-hive.com/tomizoo/pg/Subversion%20%A5%AD%A1%BC%A5%EF%A1%BC%A5%C9%A4%CE%C5%B8%B3%AB)
  * [僕の道(仮)　devlog: Subversion　クライアントの設定（TortoiseSVN）](http://bokunomichi.blogspot.com/2007/03/subversiontortoisesvn.html)
  * [ヒビノアワ: TortoiseSVNでキーワードアンカーの設定をする](http://cheebow.info/chemt/archives/2006/03/tortoisesvn.html)