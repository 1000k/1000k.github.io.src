---
title: Bower で JavaScript パッケージを管理する
author: 1000k
layout: post
date: 2013-05-04
url: /2013/05/04/managing-js-packages-by-bower/
categories:
  - JavaScript
tags:
  - Bower
  - JavaScript
  - node.js
  - チュートリアル
  - パッケージ管理
---
## Bower とは？

[BOWER: A package manager for the web](http://bower.io/)

Twitter 製の JavaScript パッケージ管理ライブラリ。Ruby の Gem, PHP の Composer みたいなもの。

プロジェクトで使用する JS ライブラリとバージョンをファイルに記述して管理できるため、バージョン管理が容易になります。

<!--more-->

## インストール

CentOS 6 でインストールする手順です。

  1. node.js をインストールする。
      * EPEL が使えれば `yum install nodejs` で一発。
  2. npm をインストールする。
      * パッケージが公開されていないので、ソースからインストールまたは `curl http://npmjs.org/install.sh | sh` (Fancy install) で。
  3. npm で bower をインストールする。
      * `sudo npm install -g bower` で終わり。

## 使い方

プロジェクトのルートに `bower.json` を作り、パッケージ情報を記述し、`bower install` を叩くのが基本です。

```
# カレントディレクトリの bower.json を参照してパッケージをインストールする
bower install

# パッケージを指定してインストールする
bower install <package>

# git にタグ付けされたバージョンを元にインストールする
bower install <package>#<version>
```


bower.json のサンプルは下記の通り。

```
{
  "name": "wpn"
  , "version": "1.0.0"
  , "main": "app/webroot/js/main.js"
  , "ignore": [
    ".jshintrc"
    ,"**/*.txt"
  ]
  , "dependencies": {
    "backbone": "1.0.0"
    , "underscore": "1.4.4"
    , "jquery": "< 2.0.0"
    , "jquery-mousewheel": "latest"
    , "snap": "git://github.com/jakiestfu/Snap.js.git"
  }
}
```


ごらんのように、Bower リポジトリに登録されているパッケージは柔軟なバージョン指定ができます。

登録されていないパッケージも、GitHub の URL を直接指定することで管理ができます。

## 好きなディレクトリにコンポーネントをインストールする

デフォルトだと bower.json と同じ階層に components/ というディレクトリを作られてしまいます。これを変えるには、bower.json と同じ階層に '.bowerrc\` を作成し、以下のように書けば OK です。

```
{
  "directory": "app/webroot/js"
}
```


これで `bower install` を実行すると、コンポーネントは `app/webroot/js/<package>` にインストールされます。

## 参考

  * [BOWER: A package manager for the web](http://bower.io/)
  * [bower/bower · GitHub](https://github.com/bower/bower)
  * [bowerでつまづいた事のメモなど - uzullaがブログ](http://uzulla.hateblo.jp/entry/2013/04/28/143507)
  * [パッケージマネージャー「Bower」が大変便利で捗りそうです | Mach3.laBlog](http://blog.mach3.jp/2013/01/bower.html)
  * [なにはともあれ入れてみるぜ。バウワー。(bower) #Node.js #JavaScript #twitter - Qiita [キータ]](http://qiita.com/items/ba952bdade627af99e93)
  * [Meet Bower: A Package Manager For The Web | Nettuts+](http://net.tutsplus.com/tutorials/tools-and-tips/meet-bower-a-package-manager-for-the-web/)