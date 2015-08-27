---
title: Protractor が動かなくてハマった
author: 1000k
layout: post
date: 2014-05-22
url: /2014/05/22/protractor-doesnt-work-for-me/
categories:
  - JavaScript
  - Node.js
  - テスト
tags:
  - Angular.js
  - node.js
  - テスト
  - トラブルシューティング
---
<a href="https://github.com/angular/angular-seed" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://github.com/angular/angular-seed', 'angular-seed']);" >angular-seed</a> を使って Angular.js の開発環境を作成していたのですが、どうにもこうにも e2e テストが動かなくてハマりました。

エラーメッセージは以下のとおり。

```
$ npm run protractor

> angular-seed@0.0.0 preprotractor /var/www
> npm run update-webdriver


> angular-seed@0.0.0 preupdate-webdriver /var/www
> npm install


> angular-seed@0.0.0 postinstall /var/www
> bower install


> angular-seed@0.0.0 update-webdriver /var/www
> webdriver-manager update

selenium standalone is up to date.
chromedriver is up to date.

> angular-seed@0.0.0 protractor /var/www
> protractor test/protractor-conf.js


------------------------------------
PID: 2314 (capability: chrome #1)
------------------------------------

Starting selenium standalone server...

events.js:72
        throw er; // Unhandled 'error' event
              ^
Error: spawn ENOENT
    at errnoException (child_process.js:998:11)
    at Process.ChildProcess._handle.onexit (child_process.js:789:34)
[launcher] Runner Process Exited With Error Code: 8

npm ERR! angular-seed@0.0.0 protractor: `protractor test/protractor-conf.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the angular-seed@0.0.0 protractor script.
npm ERR! This is most likely a problem with the angular-seed package,
npm ERR! not with npm itself.
npm ERR! Tell the author that this fails on your system:
npm ERR!     protractor test/protractor-conf.js
npm ERR! You can get their info via:
npm ERR!     npm owner ls angular-seed
npm ERR! There is likely additional logging output above.
npm ERR! System Linux 3.13.0-24-generic
npm ERR! command "node" "/usr/bin/npm" "run" "protractor"
npm ERR! cwd /var/www
npm ERR! node -v v0.10.28
npm ERR! npm -v 1.4.10
npm ERR! code ELIFECYCLE
npm ERR!
npm ERR! Additional logging details can be found in:
npm ERR!     /var/www/npm-debug.log
npm ERR! not ok code 0
```

`code ELIFECYCLE` が何を意味しているのかさっぱりわからず途方に暮れていましたが、結局のところ Java に PATH が通っていなかったのが原因だったようです。

e2e テストは内部で Selenium Standalone Server を呼んでいますが、そこに Java が必要でした。

単純に OpenJDK をパッケージインストールするだけで解決しました。

```
$ sudo apt-get install openjdk-7-jre-headless
$ java -version
(バージョンが表示されれば PATH が通ってるので OK)
```

Node.js はデバッグメッセージが雑なことが多いので苦手です。

## 参考

  * <a href="http://stackoverflow.com/questions/20188679/how-to-run-protractor/23772014#23772014" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://stackoverflow.com/questions/20188679/how-to-run-protractor/23772014#23772014', 'angularjs &#8211; How to run protractor? &#8211; Stack Overflow']);" >angularjs &#8211; How to run protractor? &#8211; Stack Overflow</a>