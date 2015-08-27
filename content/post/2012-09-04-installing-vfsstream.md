---
title: vfsStreamをインストールする
author: 1000k
layout: post
date: 2012-09-04
url: /2012/09/04/installing-vfsstream/
categories:
  - PHP
  - PHPUnit
  - テスト
tags:
  - Mock
  - PHPUnit
  - トラブルシューティング
---
PHPUnit のマニュアルに出てくる仮想ファイルシステムの vfsStream ですが、<a href="http://www.phpunit.de/manual/3.6/ja/test-doubles.html#test-doubles.mocking-the-filesystem" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.phpunit.de/manual/3.6/ja/test-doubles.html#test-doubles.mocking-the-filesystem', '公式マニュアル']);" title="第10章 テストダブル">公式マニュアル</a>に載っている方法では既にPEARチャンネルが消えているため、インストールできません。

以下に正しいインストール方法をメモしておきます。

<!--more-->

## PHP < 5.2 の場合

以下のコマンドで vfsStream < 1.0 をインストールできます。

```
$ pear channel-discover pear.bovigo.org
$ pear install bovigo/vfsStream-beta

downloading vfsStream-0.12.0.tgz ...
Starting to download vfsStream-0.12.0.tgz (459,376 bytes)
.........done: 459,376 bytes
install ok: channel://pear.bovigo.org/vfsStream-0.12.0
```


## PHP >= 5.3 の場合

vfsStream >= 1.0 を <a href="http://getcomposer.org/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://getcomposer.org/', 'Composer']);" >Composer</a> でインストールします。

```
"mikey179/vfsStream": "v1.1.0"
```


利用可能なバージョンは <a href="https://packagist.org/packages/mikey179/vfsStream" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://packagist.org/packages/mikey179/vfsStream', 'mikey179/vfsStream &#8211; Packagist']);" >mikey179/vfsStream &#8211; Packagist</a> を参照。

## 参考

  * <a href="https://github.com/mikey179/vfsStream/wiki/Install" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://github.com/mikey179/vfsStream/wiki/Install', 'Download and install vfsStream · mikey179/vfsStream Wiki']);" title="Download and install vfsStream · mikey179/vfsStream Wiki">Download and install vfsStream · mikey179/vfsStream Wiki</a>