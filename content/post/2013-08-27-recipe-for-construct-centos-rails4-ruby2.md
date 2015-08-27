---
title: CentOS+Rails4+Ruby2.0の環境を構築するレシピ
author: 1000k
layout: post
date: 2013-08-27
url: /2013/08/27/recipe-for-construct-centos-rails4-ruby2/
categories:
  - Chef
  - Ruby
  - Ruby on Rails
  - 開発環境
tags:
  - Berkshelf
  - Chef
  - Ruby
  - Ruby on Rails
  - Vagrant
---
<a href="https://github.com/1000k/rails_sandbox" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://github.com/1000k/rails_sandbox', '1000k/rails_sandbox']);" >1000k/rails_sandbox</a>

Vagrant を使って以下の環境を速攻で構築するレシピを作りました。

  * CentOS 6.4
  * Ruby 2.0.0
  * Ruby on Rails 4.0.0

使い方は <a href="https://github.com/1000k/rails_sandbox/blob/master/README.md" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://github.com/1000k/rails_sandbox/blob/master/README.md', 'README']);" >README</a> に書いてある通り、vagrant-berkshelf プラグインをインストールして `vagrant up` するだけです。

最近ようやく第2言語として Ruby が使えるようになってきたので、ここいらで Rails を使いこなせるようになりたいです。