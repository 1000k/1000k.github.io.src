---
title: 最新のパッケージをyumでインストールする
author: 1000k
layout: post
date: 2012-01-15
url: /2012/01/15/installing-up-to-date-package-by-yum/
categories:
  - Apache
  - Linux
  - PHP
tags:
  - Apache
  - Linux
  - PHP
  - yum
  - インストール
---
yum公式リポジトリ内の古いApacheやphpではなく、最新版をrpmで入れる方法です。

下記に記す非公式リポジトリを参照することで、最新のパッケージをyumでインストールできます。

  * [Remi](http://blog.famillecollet.com/)
  * [Utter Ramblings](http://www.jasonlitka.com/)

なお、これらのサイトで配布されているパッケージの信頼性は公式のリポジトリのものより劣るので、利用の際には注意が必要です。

<!--more-->

# 手順

## Remiリポジトリ追加

RemiリポジトリはEPELリポジトリに依存しているため、両方追加する必要があります。

最新のダウンロード先は下記から確認してください。

  * Remi: [RepoView: Les RPM de Remi](http://rpms.famillecollet.com/)
  * EPEL: http://download.fedora.redhat.com/pub/epel/5/i386/

```
# wget http://rpms.famillecollet.com/enterprise/remi-release-5.rpm
# wget http://download.fedora.redhat.com/pub/epel/5/i386/epel-release-5-4.noarch.rpm
# rpm -ivh epel-release-5-4.noarch.rpm remi-release-5.rpm
```


※以前のバージョンのrpmが入っている場合、「rpm -Uvh {パッケージ名}」でアップグレードします。

これでyumのリポジトリにRemiとEPELを参照できるようになりました。

```
$ ls /etc/yum.repos.d/
CentOS-Base.repo       CentOS-Media.repo  epel-testing.repo  remi.repo
CentOS-Debuginfo.repo  CentOS-Vault.repo  epel.repo
```


## Utterリポジトリ追加

**/etc/yum.repos.d/utter.repo** を開き、下記のように編集します。

```
[utter]
name=Jason's Utter Ramblings Repo
baseurl=http://www.jasonlitka.com/media/EL$releasever/$basearch/
enabled=0
gpgcheck=1
gpgkey=http://www.jasonlitka.com/media/RPM-GPG-KEY-jlitka
```


以上で、phpやhttpdの最新版がyumでインストールできるようになりました。

## 最新パッケージのインストール

yumコマンドに「-enablerepo={リポジトリ名}」オプションを付けることで、それぞれのリポジトリを参照できます。

PHPの場合

```
# yum --enablerepo=remi install php
...
===================================================================================================================
 Package                  Arch                 Version                                 Repository             Size
===================================================================================================================
Installing:
 php                      x86_64               5.3.9-1.el5.remi                        remi                  2.8 M
Installing for dependencies:
 httpd                    x86_64               2.2.3-53.el5.centos.3                   updates               1.2 M
 libedit                  x86_64               2.11-2.20080712cvs.el5                  epel                   80 k
 php-cli                  x86_64               5.3.9-1.el5.remi                        remi                  2.6 M
 php-common               x86_64               5.3.9-1.el5.remi                        remi                  997 k

Transaction Summary
===================================================================================================================
Install       5 Package(s)
Upgrade       0 Package(s)

Total download size: 7.7 M
Is this ok [y/N]:
```


Apache2(httpd)の場合

```
# yum --enablerepo=utter install httpd
...
===================================================================================================================
 Package                       Arch                   Version                          Repository             Size
===================================================================================================================
Installing:
 httpd                         x86_64                 2.2.21-jason.1                   utter                 3.2 M
Installing for dependencies:
 apr-util-ldap                 x86_64                 1.3.12-1.jason.1                 utter                  20 k
Updating for dependencies:
 apr-util                      x86_64                 1.3.12-1.jason.1                 utter                 201 k

Transaction Summary
===================================================================================================================
Install       2 Package(s)
Upgrade       1 Package(s)

Total download size: 3.4 M
Is this ok [y/N]:
```


# 参考

  * [hoge001 : CentOS5.5 Apache 2.2.15 インストール](http://hoge001.exblog.jp/13982612/)
  * [About EPEL/ja - FedoraProject](https://fedoraproject.org/wiki/About_EPEL/ja)
      * EPELとは？EPELの目的など
  * [yumでremiリポジトリを使えるようにする | グーフー WordPressのためのLinuxノート](http://www.goofoo.jp/2011/03/556)
  * [yumで、より新しいパッケージをインストールする方法(CentOS) - [yum/Linux [Red Hatなど]] ぺんたん info](http://pentan.info/server/linux/yum_new.html)