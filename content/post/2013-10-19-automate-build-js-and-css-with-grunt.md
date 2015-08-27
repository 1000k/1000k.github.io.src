---
title: Grunt で JS/CSS を自動ビルドする
author: 1000k
layout: post
date: 2013-10-19
url: /2013/10/19/automate-build-js-and-css-with-grunt/
categories:
  - JavaScript
  - 開発環境
tags:
  - Grunt
  - JavaScript
  - node.js
  - npm
---
## Grunt とは？

JS/CSS のビルド自動化ツール。

JS のユニットテスト、ファイルの結合＆難読化＆最小化などの様々なタスクを自動で行うことができます。

## インストール

まず、node.js と npm をインストールします。

やり方は以下の記事あたりを参考に。

  * <a href="http://havelog.ayumusato.com/develop/javascript/e210-install_nodejs_and_npm.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://havelog.ayumusato.com/develop/javascript/e210-install_nodejs_and_npm.html', 'node.jsとnpmのインストールをしたメモ（CentOS さくらのVPS） ::ハブろぐ']);" >node.jsとnpmのインストールをしたメモ（CentOS さくらのVPS） ::ハブろぐ</a>

次に grunt-cli をグローバルにインストールします。

```
$ sudo npm install -g grunt-cli
```


次に、プロジェクトのトップに `package.json` を作成します。

この中には、使用したい Grunt ライブラリを列挙します。

```
{
  "name": "mojamoja-project",
  "version": "0.1.0",
  "devDependencies": {
    "grunt": "~0.4.1",
    "grunt-contrib-sass": "~0.3.0",
    "grunt-contrib-jshint": "~0.6.3",
    "grunt-contrib-nodeunit": "~0.2.0",
    "grunt-contrib-compass": "~0.2.0",
    "grunt-contrib-concat": "~0.1.2",
    "grunt-contrib-qunit": "~0.1.1",
    "grunt-contrib-uglify": "~0.2.2",
    "grunt-contrib-watch": "~0.2.0"
  }
}
```


これで `npm install grunt --save-dev` を叩くと、package.json に書かれたパッケージが一気にインストールされます。

以上で前準備は完了です。

## 使い方

プロジェクトのルートに `Gruntfile.js` を書いて、実行したいタスクを列挙するだけです。

※ grunt < 0.4 では `grunt.js` というファイル名でした。

```
module.exports = function(grunt) {
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    concat: {
      options: {
        separator: ';'
      },
      dist: {
        src: [
          'app/webroot/js/vendor/jquery/jquery.js',
          'app/webroot/js/vendor/jquery-mousewheel/jquery.mousewheel.js',
          'app/webroot/js/vendor/underscore/underscore.js',
          'app/webroot/js/vendor/backbone/backbone.js',
          'app/webroot/js/vendor/backbone.localStorage/backbone.localStorage.js',
          'app/webroot/js/vendor/hook/hook.js',
          'app/webroot/js/vendor/mobify-modules/carousel/src/carousel.js',
          'app/webroot/js/vendor/snap/snap.js',
          'app/webroot/js/app.js'
        ],
        dest: 'app/webroot/js/merged.js'
      }
    },
    uglify: {
      options: {
        banner: '/*! &lt;%= pkg.name %> &lt;%= grunt.template.today("dd-mm-yyyy") %> */\n'
      },
      dist: {
        files: {
          'app/webroot/js/merged.min.js': ['&lt;%= concat.dist.dest %>']
        }
      }
    },
    jshint: {
      files: ['gruntfile.js', 'app/webroot/js/**/*.js'],
      options: {
        // options here to override JSHint defaults
        globals: {
          jQuery: true,
          console: true,
          module: true,
          document: true
        }
      }
    },
    watch: {
      files: ['&lt;%= jshint.files %>'],
      tasks: ['concat', 'uglify']
    }
  });

  grunt.loadNpmTasks('grunt-contrib-uglify');
  grunt.loadNpmTasks('grunt-contrib-jshint');
  grunt.loadNpmTasks('grunt-contrib-qunit');
  grunt.loadNpmTasks('grunt-contrib-watch');
  grunt.loadNpmTasks('grunt-contrib-concat');

  grunt.registerTask('test', ['jshint']);
  grunt.registerTask('default', ['jshint', 'concat', 'uglify']);
  grunt.registerTask('merge', ['concat', 'uglify']);

};
```


詳しい記述方法は公式のガイドを参考に。

  * <a href="http://gruntjs.com/configuring-tasks" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://gruntjs.com/configuring-tasks', 'Configuring tasks &#8211; Grunt: The JavaScript Task Runner']);" >Configuring tasks &#8211; Grunt: The JavaScript Task Runner</a>
  * <a href="http://gruntjs.com/creating-tasks" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://gruntjs.com/creating-tasks', 'Creating tasks &#8211; Grunt: The JavaScript Task Runner']);" >Creating tasks &#8211; Grunt: The JavaScript Task Runner</a>

あとは `grunt {タスク名}` と叩くだけで、指定したタスクが動きます。

例えば上で定義 (`grunt.registerTask()` )したタスクから merge を実行したければ、`grunt merge` と叩けば OK です。。

## 変更の監視＆自動タスク実行

`grunt watch` を叩くことで、指定した js ファイルを監視し、変更があったら自動で指定したタスクを実行してくれます。

## 参考

  * <a href="http://hamalog.tumblr.com/post/18137176043/grunt-js-css" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://hamalog.tumblr.com/post/18137176043/grunt-js-css', 'gruntで快適JS/CSSビルド生活 &#8211; Takazudo hamalog']);" >gruntで快適JS/CSSビルド生活 &#8211; Takazudo hamalog</a>
  * <a href="http://gruntjs.com/getting-started" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://gruntjs.com/getting-started', 'Getting started &#8211; Grunt: The JavaScript Task Runner']);" >Getting started &#8211; Grunt: The JavaScript Task Runner</a>
  * <a href="https://github.com/gruntjs/grunt/wiki/Sample-Gruntfile" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://github.com/gruntjs/grunt/wiki/Sample-Gruntfile', 'Sample Gruntfile · gruntjs/grunt Wiki']);" >Sample Gruntfile · gruntjs/grunt Wiki</a>
      * Gruntfile のサンプル。
  * <a href="http://gruntjs.com/plugins/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://gruntjs.com/plugins/', 'Plugins &#8211; Grunt: The JavaScript Task Runner']);" >Plugins &#8211; Grunt: The JavaScript Task Runner</a>
      * grunt プラグインの一覧。
      * 公式のものは &#8216;Show contrib plugins first&#8217; にチェックを入れれば出てくる。
  * <a href="http://publicdraft.studiomohawk.com/2012/12/16/npm-and-package.json/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://publicdraft.studiomohawk.com/2012/12/16/npm-and-package.json/', 'npm / package.json | Public Draft | Outlining and Researching for the Presentation']);" >npm / package.json | Public Draft | Outlining and Researching for the Presentation</a>
      * package.json の基本。