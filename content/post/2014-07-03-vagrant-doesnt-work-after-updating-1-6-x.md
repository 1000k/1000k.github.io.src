---
title: Vagrant 1.6.x にアップデートしたら動かなくなった
author: 1000k
layout: post
date: 2014-07-03
url: /2014/07/03/vagrant-doesnt-work-after-updating-1-6-x/
categories:
  - 仮想化
  - 開発環境
tags:
  - Vagrant
  - トラブルシューティング
---
Windows 環境で Docker が使いたくて Vagrant を 1.5.1 から 1.6.3 にアップグレードしたら、`vagrant up` するたびにエラーが出るようになってしましました。

```
$ vagrant up
Bringing machine 'myapp' up with 'virtualbox' provider...
==> myapp: Box 'myapp' could not be found. Attempting to find and
install...
    myapp: Box Provider: virtualbox
    myapp: Box Version: >= 0
==> myapp: Adding box 'myapp' (v0) for provider: virtualbox
    myapp: Downloading: file://C:/static/boxes/CentOS-6.3-x86_64-v2013010
1.box
    myapp: Progress: 8% (Rate: 201M/s, Estimated time remaining: 0:00:02)
    myapp: Progress: 53% (Rate: 203M/s, Estimated time remaining: 0:00:01
    myapp: Progress: 56% (Rate: 108M/s, Estimated time remaining: 0:00:01
    myapp: Progress: 76% (Rate: 103M/s, Estimated time remaining: 0:00:01
    myapp: Progress: 89% (Rate: 91.7M/s, Estimated time remaining: --:--:
    myapp: Progress: 100% (Rate: 85.8M/s, Estimated time remaining: --:--
    myapp:
The box failed to unpackage properly. Please verify that the box
file you're trying to add is not corrupted and try again. The
output from attempting to unpackage (if any):
```

GitHub の issue レポートにワークアラウンドが見つかりました。

<a href="https://github.com/mitchellh/vagrant/issues/3674" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://github.com/mitchellh/vagrant/issues/3674', 'The executable &#8216;bsdtar&#8217; Vagrant is trying to run was not found in the %PATH% variable · Issue #3674 · mitchellh/vagrant']);" >The executable &#8216;bsdtar&#8217; Vagrant is trying to run was not found in the %PATH% variable · Issue #3674 · mitchellh/vagrant</a>

これによると、古いバージョンが入った Windows に 1.6.x をインストールすると、bsdtar (Vagrant Box を解凍するツール？) がなぜかインストールされないそうです。実際に `C:\HashiCorp\Vagrant\embedded\gnuwin32\bin` を見てみたところ、`bsdtar.exe` は見つからず、`libarchive.dll` というファイル1つしかありませんでした。

そんなわけで以下の手順を行ったところ、無事起動するようになりました。

  1. 古い Vagrant をアンインストールする。
  2. `c:\HashiCorp\` を削除する。
  3. 再度インストーラーで Vagrant 1.6.3 をインストールする。

なおスレッドの書き込みによると、コントロールパネルの「プログラムと機能」から「修復」を選んでも直るらしいです。(未検証)