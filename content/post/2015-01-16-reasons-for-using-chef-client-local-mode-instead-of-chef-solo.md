---
title: Chef Solo ではなく Chef Client Local Mode を使おう
author: 1000k
layout: post
date: 2015-01-16
url: /2015/01/16/reasons-for-using-chef-client-local-mode-instead-of-chef-solo/
categories:
  - Chef
  - 開発環境
tags:
  - Chef
  - Vagrant
---
2014/06/24 に Opscode 公式ブログで <a href="http://www.getchef.com/blog/2014/06/24/from-solo-to-zero-migrating-to-chef-client-local-mode/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.getchef.com/blog/2014/06/24/from-solo-to-zero-migrating-to-chef-client-local-mode/', 'From Solo to Zero: Migrating to Chef Client Local Mode']);" >From Solo to Zero: Migrating to Chef Client Local Mode</a> という記事が公開されました。記事を要約すると「Chef Solo はオワコンだから Chef Client Local Mode を使え」ということのようです。

## Chef Client Local Mode (旧 Chef Zero) って？

Chef Client Local Mode (旧 Chef Zero) は Chef Solo の全ての機能を備えており、Chef Server に移行する際にも楽です。Chef Client 11.8.0 から採用されました。実行するには `chef-client` に `--local-mode` または `-z` オプションを付けます。

Local Mode ではローカルのファイルシステムから揮発性の Chef Server を作り、

Server-Client 構成と同じオペレーションが行われます。つまり Chef Server が無ければできなかった高度な機能を、サーバーレスで気軽に行うことができます。

Chef Server のフル機能を、Chef Solo 同様の気軽さで使えるのが、Chef Client Local Mode と言っていいでしょう。

## Chef Solo から Chef Client Local mode に移行するべき理由

1&#46; Local Mode なら Server-Client で使える全ての機能 &#8212; environments, roles, (encrypted) data bags &#8212; が使える。ノードやレシピの検索機能も使える。

2&#46; 特別なツールや knife プラグインは必要無い。

3&#46; レシピ実行環境が Solo か Server かの分岐を書く必要が無くなる。例えば以下のようなコード。

```
if Chef::Config[:solo]
    # do something
else
    # do something else
end
```

4&#46; `chef_zero` を Test Kitchen の provisioner に使う際、 &#8220;fixture node objects&#8221; をテストデータとして使える。

5&#46; 後に Chef Server 構成を使いたくなった時にすんなり移行できる。

今すぐ Chef Solo をお払い箱にする必要はありませんが、公式に deprecated 扱いされているので、今後は Solo よりも Local Mode を使うことをお薦めします。これから Chef を始める人は、chef-apply か local mode を使うといいでしょう。

## Vagrant で使う時はどうする？

Vagrant 1.7.0 より、Chef Client Local mode が使えるようになりました。単純に provisioner に `chef_zero` を指定するだけで動作します。

<a href="https://docs.vagrantup.com/v2/provisioning/chef_zero.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://docs.vagrantup.com/v2/provisioning/chef_zero.html', 'Chef Zero &#8211; Provisioning &#8211; Vagrant Documentation']);" >Chef Zero &#8211; Provisioning &#8211; Vagrant Documentation</a>

```ruby
Vagrant.configure("2") do |config|
  config.vm.provision "chef_zero" do |chef|
    # Specify the local paths where Chef data is stored
    chef.cookbooks_path = "cookbooks"
    chef.roles_path = "roles"
    chef.nodes_path = "nodes"

    # Add a recipe
    chef.add_recipe "apache"

    # Or maybe a role
    chef.add_role "web"
  end
end
```

## 参考

  * <a href="http://www.getchef.com/blog/2014/06/24/from-solo-to-zero-migrating-to-chef-client-local-mode/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.getchef.com/blog/2014/06/24/from-solo-to-zero-migrating-to-chef-client-local-mode/', 'From Solo to Zero: Migrating to Chef Client Local Mode | Chef Blog']);" >From Solo to Zero: Migrating to Chef Client Local Mode | Chef Blog</a>
  * <a href="http://www.slideshare.net/YukihikoSawanobori/chef-2014" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.slideshare.net/YukihikoSawanobori/chef-2014', '2014年のChefとInfrastructure as code']);" >2014年のChefとInfrastructure as code</a>
  * <a href="http://docs.opscode.com/ctl_chef_client.html#run-in-local-mode" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://docs.opscode.com/ctl_chef_client.html#run-in-local-mode', 'chef-client (executable) — Chef Docs # Run in Local Mode']);" >chef-client (executable) — Chef Docs # Run in Local Mode</a>
  * <a href="http://docs.opscode.com/ctl_chef_solo.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://docs.opscode.com/ctl_chef_solo.html', 'chef-solo (executable) — Chef Docs']);" >chef-solo (executable) — Chef Docs</a>
      * 一番上に warning で「`--local-mode` を検討しなよ」と書いてある。