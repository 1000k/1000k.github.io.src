---
title: XAMPP環境にJenkinsをインストールする
author: 1000k
layout: post
date: 2012-09-25
url: /2012/09/25/installing-jenkins-in-xampp-env/
categories:
  - CI
  - Jenkins
tags:
  - Jenkins
  - Windows
  - XAMPP
  - チュートリアル
---
XAMPPにはTomcatが同梱されているため、JenkinsのWARファイルをデプロイすれば、すぐにCI環境が構築できます。

以下、[XAMPP for Windows 1.8.0](http://www.apachefriends.org/jp/xampp-windows.html) を使ってJenkins環境を構築する手順を書きます。

<!--more-->

## 手順

### Jenkinsをダウンロードする

[{{< img src="/img/jenkins_homepage.png" title="" >}}](http://blog.1000k.net/wp-content/uploads/jenkins_homepage.png)

[Jenkinsのホームページ](http://jenkins-ci.org/)から最新のwarアーカイブをダウンロードします。「Latest and greatest」をクリックして jenkins.war をダウンロードしてください。**Windowsパッケージではない**ので注意してください。

### Tomcatの管理者ユーザーを設定する

Tomcatを起動する前に、Tomcat管理画面に入るためのユーザーを作成します。

_C:\xampp\tomcat\conf\tomcat-users.xml_ を開いて、tomcat-usersブロック内に以下の2行を追加してください。

<div>
  [xml]<br /> <tomcat-users><br /> <role rolename="manager-gui"/><br /> <user username="tomcat" password="s3cret" roles="manager-gui"/><br /> </tomcat-users><br /> [/xml]
</div>

### Tomcatを起動する

[{{< img src="/img/xampp_control_panel.png" title="" >}}](http://blog.1000k.net/wp-content/uploads/xampp_control_panel.png)

XAMPPコントロールパネルを起動し、Tomcatの横にある「Start」ボタンをクリックするだけです。

ボタンが「Stop」に変わった後 <http://localhost:8080/> にアクセスすれば、Tomcatのトップ画面が表示されます。

### 管理画面にログインする

[{{< img src="/img/tomcat_top.png" title="" >}}](http://blog.1000k.net/wp-content/uploads/tomcat_top.png)

「Manage App」をクリックして管理画面にログインします。認証パスワードは先ほど設定した通り、ユーザー名が「_tomcat_」、パスワードが「_s3cret_」となります。

### warファイルをデプロイする

[{{< img src="/img/tomcat_manage.png" title="" >}}](http://blog.1000k.net/wp-content/uploads/tomcat_manage.png)

「warファイルの配備」に先ほどダウンロードした jenkins.war を指定し、「配備」をクリックします。

### Jenkinsの画面を確認する

[{{< img src="/img/jenkins_dashboard.png" title="" >}}](http://blog.1000k.net/wp-content/uploads/jenkins_dashboard.png)

以上で完了です。<http://localhost:8080/jenkins/> にアクセスすれば Jenkins のトップ画面が表示されます。