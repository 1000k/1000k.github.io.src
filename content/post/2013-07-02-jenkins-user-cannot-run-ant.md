---
title: jenkins ユーザーが ant を実行できないときの対処法
author: 1000k
layout: post
date: 2013-07-02
url: /2013/07/02/jenkins-user-cannot-run-ant/
categories:
  - CI
  - Jenkins
  - PHP
  - PHPUnit
  - テスト
tags:
  - CentOS
  - CI
  - Jenkins
  - PHPUnit
  - トラブルシューティング
---
CentOS 6.3 にて、<a href="http://jenkins-php.org/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://jenkins-php.org/', 'Template for Jenkins Jobs for PHP Projects']);" >Template for Jenkins Jobs for PHP Projects</a> にある ant タスク (build.xml) 通りに PHPUnit タスクを実行しようとしたら、エラーが出てしまいました。

かなり長時間ハマったのでメモを残しておきます。

<!--more-->

## 症状

CentOS 6.3 にて、<a href="http://jenkins-php.org/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://jenkins-php.org/', 'Template for Jenkins Jobs for PHP Projects']);" >Template for Jenkins Jobs for PHP Projects</a> にある以下のタスクを実行するとエラーが出ました。```
 <target name="phpunit" description="Run unit tests using PHPUnit and generates junit.xml and clover.xml"> <exec executable="phpunit" failonerror="true"/> </target> ```


エラーメッセージ:```
 # sudo -u jenkins ant ... phpunit: BUILD FAILED /var/lib/jenkins/jobs/mojamoja-unit-testing/workspace/build.xml:22: Execute failed: java.io.IOException: Cannot run program "phpunit": error=2, No such file or directory Total time: 0 seconds ```


## 原因

どうやら phpunit へのパスが通っていないようです。

phpunit の実体は `/usr/local/bin/phpunit` なのですが、jenkins ユーザーがこの PATH を参照できていない模様。

## 試行錯誤

まず、jenkins ユーザーの `~/.bash_profile` と `~/.bashrc` を以下の内容で作成してもダメでした。PATH は反映されません。

```

export PATH=$PATH:/usr/local/bin
```

<p>また、下記のように ant タスク内で PATH を変更するシェルを走らせてもダメでした。</p>
```
&lt;target name="setenv">
   &lt;exec executable="/bin/sh">
     &lt;arg line="./setenv.sh"/>
   &lt;/exec>
 &lt;/target>

 &lt;target name="env">&lt;exec executable="env" />&lt;/target>
```


setenv.sh の中身は下記。

```
#!/bin/sh
export PATH=${PATH}:/usr/local/bin
echo $PATH
```


これで ant を実行すると、`setenv` タスクの時点では PATH は変更されていますが、次に実行される `env` タスクで元に戻ってしまっています。まるで setenv と env が別々のプロセスで動いているように見えます。

```
Buildfile: build.xml

setenv:
     [exec] /sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin

env:
     [exec] HOSTNAME=mojamoja
     [exec] SHELL=/bin/bash
     [exec] TERM=xterm
     [exec] HISTSIZE=1000
     [exec] USER=jenkins
     ...
     [exec] PATH=/sbin:/bin:/usr/sbin:/usr/bin      &lt;- /usr/local/bin が追加されていない
```


とういわけで、setenv で環境変数をセットする方法はボツでした。

## 解決方法

### 1. Jenkins のグローバルプロパティを設定する

Jenkins から環境変数を上書きすることで、PATH を追加することができます。

`Jenkinsの管理 > システムの設定 > グローバルプロパティ > 環境変数` にて、以下のように設定します。

<a href="http://blog.1000k.net/wp-content/uploads/jenkins_global_property.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/jenkins_global_property.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/jenkins_global_property-300x191.png" alt="Jenkins グローバルプロパティ" width="300" height="191" class="alignnone size-medium wp-image-1507" /></a>

  * キー: `PATH`
  * 値: `${PATH}:/usr/local/bin`

ただし、この方法だと Jenkins から Ant を実行した時しか PATH は上書きされません。

### 2. sudoers の secure_path を無効にする

Fedora 系では sudo ユーザー向けの PATH が `/sbin:/bin:/usr/sbin:/usr/bin` のみに制限されているので、これを外してやります。

`visudo` コマンドを実行して、以下のように書き直します。```
 # secure\_path をコメントアウト # Defaults secure\_path = /sbin:/bin:/usr/sbin:/usr/bin # env\_keep に PATH を追加 Defaults env\_keep += "PATH" ```


これで secure_path の縛りが解除されました。

sudoers の環境変数は \`sudo -l\` で確認できます。```
 # sudo -l Matching Defaults entries for root on this host: !visiblepw, always\_set\_home, env\_reset, env\_keep="COLORS DISPLAY HOSTNAME HISTSIZE INPUTRC KDEDIR LS\_COLORS", env\_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC\_ADDRESS LC\_CTYPE", env\_keep+="LC\_COLLATE LC\_IDENTIFICATION LC\_MEASUREMENT LC\_MESSAGES", env\_keep+="LC\_MONETARY LC\_NAME LC\_NUMERIC LC\_PAPER LC\_TELEPHONE", env\_keep+="LC\_TIME LC\_ALL LANGUAGE LINGUAS \_XKB\_CHARSET XAUTHORITY", env_keep+=PATH User root may run the following commands on this host: (ALL) ALL ```


## 参考

  * <a href="http://deginzabi163.wordpress.com/2011/04/23/%E8%A6%9A%E6%9B%B8fedora-14%E3%81%AEsudo%E3%81%8Cpath%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0%E3%82%92%E6%BD%B0%E3%81%99%E3%81%AE%E3%81%8C%E9%AC%B1%E9%99%B6%E3%81%97%E3%81%84%EF%BC%86%E5%AF%BE%E7%AD%96/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://deginzabi163.wordpress.com/2011/04/23/%E8%A6%9A%E6%9B%B8fedora-14%E3%81%AEsudo%E3%81%8Cpath%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0%E3%82%92%E6%BD%B0%E3%81%99%E3%81%AE%E3%81%8C%E9%AC%B1%E9%99%B6%E3%81%97%E3%81%84%EF%BC%86%E5%AF%BE%E7%AD%96/', '[覚書]Fedora 14のsudoでPATHがアレで鬱陶しい＆対策 | Deginzabi163\'s Blog']);" >[覚書]Fedora 14のsudoでPATHがアレで鬱陶しい＆対策 | Deginzabi163's Blog</a>
  * <a href="http://fishrimper.blogspot.jp/2012/12/sudo-path.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://fishrimper.blogspot.jp/2012/12/sudo-path.html', 'IT とかその他もろもろ: sudo した時の PATH']);" >IT とかその他もろもろ: sudo した時の PATH</a>
  * <a href="http://superuser.com/questions/98686/passing-path-through-sudo" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://superuser.com/questions/98686/passing-path-through-sudo', 'linux - Passing PATH through sudo - Super User']);" >linux - Passing PATH through sudo - Super User</a>