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
# 目的

&#8220;server001&#8221;, &#8220;server002&#8221;, &#8230;, &#8220;serverN&#8221; すべてで共通の httpd.conf を使い、それぞれのログファイルを &#8220;log/server001/access\_log&#8221;, &#8230;, &#8220;log/serverN/access\_log&#8221; に吐くようにしたい。

ノード追加の度に手動でconfファイルをいちいち書き換えるのにはもううんざり。

そんな人には以下の方法をおすすめします。

# やり方

Apacheディレクトリ内の **envvars** というファイルに記述することで、OSの環境変数を conf ファイルに渡すことができることを利用します。

1&#46; &#8220;apache2/bin/envvars&#8221; に以下の2行を追加する

```
HOSTNAME="$HOSTNAME"
export HOSTNAME
```


2&#46; &#8220;apache2/conf/httpd.conf&#8221; のログ出力行を編集する

```
#ErrorLog "logs/error_log"
ErrorLog "logs/${HOSTNAME}_error_log"

#CustomLog "logs/access_log" common
CustomLog "logs/${HOSTNAME}_access_log" common
```


3&#46; Apacheを再起動する

# 参考

  * <a href="http://wall-climb.com/2009/05/29/httpd-conf%E5%86%85%E3%81%A7%E5%A4%89%E6%95%B0%E3%82%92%E5%88%A9%E7%94%A8%E3%81%99%E3%82%8B/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://wall-climb.com/2009/05/29/httpd-conf%E5%86%85%E3%81%A7%E5%A4%89%E6%95%B0%E3%82%92%E5%88%A9%E7%94%A8%E3%81%99%E3%82%8B/', 'wall-climb » httpd.conf内で変数を利用する']);" title="wall-climb » httpd.conf内で変数を利用する">wall-climb » httpd.conf内で変数を利用する</a>
  * <a href="http://playispeace.com/blog/766/load_mod_env_for_use_setenv" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://playispeace.com/blog/766/load_mod_env_for_use_setenv', 'apacheのSetEnvを利用するときはmod_env.soを読み込む » pblo']);" title="apacheのSetEnvを利用するときはmod_env.soを読み込む » pblo">apacheのSetEnvを利用するときはmod_env.soを読み込む » pblo</a>