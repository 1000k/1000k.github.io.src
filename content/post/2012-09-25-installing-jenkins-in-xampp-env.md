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

以下、<a href="http://www.apachefriends.org/jp/xampp-windows.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.apachefriends.org/jp/xampp-windows.html', 'XAMPP for Windows 1.8.0']);" title="apache friends - xampp for windows">XAMPP for Windows 1.8.0</a> を使ってJenkins環境を構築する手順を書きます。

<!--more-->

## 手順

### Jenkinsをダウンロードする

<a href="http://blog.1000k.net/wp-content/uploads/jenkins_homepage.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/jenkins_homepage.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/jenkins_homepage.png" alt="" title="Jenkinsホームページ" width="600" height="553" class="alignnone size-full wp-image-1096" /></a>

<a href="http://jenkins-ci.org/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://jenkins-ci.org/', 'Jenkinsのホームページ']);" >Jenkinsのホームページ</a>から最新のwarアーカイブをダウンロードします。「Latest and greatest」をクリックして jenkins.war をダウンロードしてください。**Windowsパッケージではない**ので注意してください。

### Tomcatの管理者ユーザーを設定する

Tomcatを起動する前に、Tomcat管理画面に入るためのユーザーを作成します。

_C:\xampp\tomcat\conf\tomcat-users.xml_ を開いて、tomcat-usersブロック内に以下の2行を追加してください。

<div>
  [xml]<br /> <tomcat-users><br /> <role rolename="manager-gui"/><br /> <user username="tomcat" password="s3cret" roles="manager-gui"/><br /> </tomcat-users><br /> [/xml]
</div>

### Tomcatを起動する

<a href="http://blog.1000k.net/wp-content/uploads/xampp_control_panel.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/xampp_control_panel.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/xampp_control_panel.png" alt="" title="XAMPPコントロールパネル" width="714" height="450" class="alignnone size-full wp-image-1086" /></a>

XAMPPコントロールパネルを起動し、Tomcatの横にある「Start」ボタンをクリックするだけです。
  
ボタンが「Stop」に変わった後 <http://localhost:8080/> にアクセスすれば、Tomcatのトップ画面が表示されます。

### 管理画面にログインする

<a href="http://blog.1000k.net/wp-content/uploads/tomcat_top.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/tomcat_top.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/tomcat_top.png" alt="" title="Tomcatトップページ" width="600" height="433" class="alignnone size-full wp-image-1087" /></a>

「Manage App」をクリックして管理画面にログインします。認証パスワードは先ほど設定した通り、ユーザー名が「_tomcat_」、パスワードが「_s3cret_」となります。

### warファイルをデプロイする

<a href="http://blog.1000k.net/wp-content/uploads/tomcat_manage.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/tomcat_manage.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/tomcat_manage.png" alt="" title="Tomcatデプロイ" width="600" height="174" class="alignnone size-full wp-image-1088" /></a>

「warファイルの配備」に先ほどダウンロードした jenkins.war を指定し、「配備」をクリックします。

### Jenkinsの画面を確認する

<a href="http://blog.1000k.net/wp-content/uploads/jenkins_dashboard.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/jenkins_dashboard.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/jenkins_dashboard.png" alt="" title="Jenkinsダッシュボード" width="600" height="271" class="alignnone size-full wp-image-1090" /></a>

以上で完了です。<http://localhost:8080/jenkins/> にアクセスすれば Jenkins のトップ画面が表示されます。