---
title: MariaDB 10.0.1 を CentOS 6 にインストールする手順
author: 1000k
layout: post
date: 2013-04-14
url: /2013/04/14/tutorial-for-installing-mariadb-10-0-1-in-centos6/
categories:
  - MariaDB
tags:
  - CentOS
  - Chef
  - MariaDB
  - チュートリアル
---
yum を使ってインストールする手順です。

<!--more-->

## RPG-GPG-KEY を追加する

```
$ sudo rpm --import https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
```


## リポジトリを追加する

バージョンによって異なります。<a href="https://downloads.mariadb.org/mariadb/repositories/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://downloads.mariadb.org/mariadb/repositories/', 'MariaDB &#8211; Setting up MariaDB Repositories &#8211; MariaDB']);" >MariaDB &#8211; Setting up MariaDB Repositories &#8211; MariaDB</a> からバージョン＆環境毎のリポジトリが選択できるので、10.0 以外をインストールする場合はそちらを参考にしてください。

今回は CentOS 6.3 64bit です。

/etc/yum.repos.d/MariaDB.repo を下記の内容で作成します。

```
# MariaDB 10.0 CentOS repository list - created 2013-04-14 07:02 UTC
# http://mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.0/centos6-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```


## インストール

```
$ sudo yum install MariaDB-devel MariaDB-client MariaDB-server
```


## (おまけ) Chef レシピ

インストールするだけの手順をレシピにすると下記のようになります。

```
yum_key 'RPM-GPG-KEY-MariaDB' do
  url 'https://yum.mariadb.org/RPM-GPG-KEY-MariaDB'
  action :add
end

yum_repository 'MariaDB' do
  repo_name 'MariaDB'
  url 'http://yum.mariadb.org/10.0/centos6-amd64'
  key 'RPM-GPG-KEY-MariaDB'
  action :create
end

package 'MariaDB-devel'

package 'MariaDB-client'

package 'MariaDB-server'
```


## 参考

  * <a href="https://downloads.mariadb.org/mariadb/repositories/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://downloads.mariadb.org/mariadb/repositories/', 'MariaDB &#8211; Setting up MariaDB Repositories &#8211; MariaDB']);" >MariaDB &#8211; Setting up MariaDB Repositories &#8211; MariaDB</a>
  * <a href="https://kb.askmonty.org/en/installing-mariadb-with-yum/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://kb.askmonty.org/en/installing-mariadb-with-yum/', 'Installing MariaDB with yum &#8211; AskMonty KnowledgeBase']);" >Installing MariaDB with yum &#8211; AskMonty KnowledgeBase</a>
  * <a href="http://www.e-agency.co.jp/column/20130208.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.e-agency.co.jp/column/20130208.html', 'MariaDBをCentOS 6にyumでインストールする方法 | ブログ | 株式会社イー・エージェンシー']);" >MariaDBをCentOS 6にyumでインストールする方法 | ブログ | 株式会社イー・エージェンシー</a>
  * <a href="https://github.com/opscode-cookbooks/yum/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://github.com/opscode-cookbooks/yum/', 'opscode-cookbooks/yum · GitHub']);" >opscode-cookbooks/yum · GitHub</a>