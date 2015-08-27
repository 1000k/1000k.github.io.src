---
title: Chef Server が FQDN を取得できない時の対処法
author: 1000k
layout: post
date: 2013-07-05
url: /2013/07/05/chef-server-cannot-get-fqdn/
categories:
  - Chef
tags:
  - Chef
  - トラブルシューティング
---
どういうわけか会社で使っている Chef Server で `knife bootstrap` した後、FQDN が空になっているノードがありました。

```
[root@chefserver-vm ~]# knife node show mojamoja-vm
Node Name:   mojamoja-vm
Environment: _default
FQDN:
IP:          172.16.83.86
Run List:    recipe[common], role[web]
Roles:       web
Recipes:     common, apache2, php
Platform:    centos 6.3
Tags:
```


これのせいで `knife search` など使う時に大変不便でした。

対処方法をメモしておきます。

<!--more-->

## 原因

クライアント側サーバーの /etc/hosts で自分の IP と hostname を定義していなかったためでした。

この状態だと、クライアント側で `hostname -A` を叩いても空の文字列が返ってきます。

```
[root@mojamoja-vm ~]# hostname -A

(↑空)
```


ohai コマンドを叩いても、やはり FQDN は取得できません。

```
# ohai | grep fqdn

(↑空)
```


## 対処法

`/etc/hosts` に IP と hostname の対を追加してやります。

```
172.16.83.86    mojamoja-vm
```


これで FQDN が取得できるようになりました。

```
[root@mojamoja-vm ~]# hostname -A
mojamoja-vm

[root@mojamoja-vm ~]# ohai | grep fqdn
  "fqdn": "mojamoja-vm",
```


サーバー側からも FQDN が取得できています。

※FQDN をサーバーに反映するために、クライアント側で一度 `chef-client` を叩く必要があるようです。

```
[root@chefserver-vm ~]# knife node show mojamoja-vm
Node Name:   mojamoja-vm
Environment: _default
FQDN:        mojamoja-vm
IP:          172.16.83.86
Run List:    recipe[common], role[web]
Roles:       web
Recipes:     common, apache2, php
Platform:    centos 6.3
Tags:
```


## 参考

  * [Setting the hostname: FQDN or short name? - Server Fault](http://serverfault.com/questions/331936/setting-the-hostname-fqdn-or-short-name)
  * [How to change hostname or FQDN in CentOS and Redhat](http://sharadchhetri.com/2012/12/23/change-hostname-fqdn-centos-redhat/)

photo by: [Chef Mauro Sousa e Sous Chef Valdir Ramos by manoel petry, on Flickr](http://www.flickr.com/photos/manoelpetry/5334219183/)