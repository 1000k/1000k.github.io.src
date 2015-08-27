---
title: '[Apache] ログを$HOSTNAME毎に出し分けする'
author: 1000k
layout: post
date: 2010-05-19
url: /2010/05/19/output-apache-log-by-hostname/
categories:
  - Apache
tags:
  - Apache
---
目的
----

"server001", "server002", …, "serverN" すべてで共通の httpd.conf を使い、それぞれのログファイルを "log/server001/access\_log", …, "log/serverN/access\_log" に吐くようにしたい。

ノード追加の度に手動でconfファイルをいちいち書き換えるのにはもううんざり。

そんな人には以下の方法をおすすめします。

やり方
----

Apacheディレクトリ内の **envvars** というファイルに記述することで、OSの環境変数を conf ファイルに渡すことができることを利用します。

1. "apache2/bin/envvars" に以下の2行を追加する

```
HOSTNAME="$HOSTNAME"
export HOSTNAME
```


2. "apache2/conf/httpd.conf" のログ出力行を編集する

```
#ErrorLog "logs/error_log"
ErrorLog "logs/${HOSTNAME}_error_log"

#CustomLog "logs/access_log" common
CustomLog "logs/${HOSTNAME}_access_log" common
```


3. Apacheを再起動する

参考
----

  * [wall-climb » httpd.conf内で変数を利用する](http://wall-climb.com/2009/05/29/httpd-conf%E5%86%85%E3%81%A7%E5%A4%89%E6%95%B0%E3%82%92%E5%88%A9%E7%94%A8%E3%81%99%E3%82%8B/)
  * [apacheのSetEnvを利用するときはmod_env.soを読み込む » pblo](http://playispeace.com/blog/766/load_mod_env_for_use_setenv)