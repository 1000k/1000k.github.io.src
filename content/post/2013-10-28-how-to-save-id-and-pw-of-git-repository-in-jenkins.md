---
title: Jenkins で Git リポジトリの ID と PW をセキュアに保存する方法
author: 1000k
layout: post
date: 2013-10-28
url: /2013/10/28/how-to-save-id-and-pw-of-git-repository-in-jenkins/
categories:
  - 開発環境
tags:
  - git
  - Jenkins
  - チュートリアル
  - トラブルシューティング
---
Jenkins から https 接続で認証付きの Git リポジトリを参照する場合、以下のように URL に直接ユーザー名とパスワードを書く必要がありました。

```
https://{USERNAME}:{PASSWORD}@foo.com/git/bar.git
```


当然 Jenkins の設定ファイルにパスワードがそのまま書かれてしまいます。個人開発ならいいですが、チーム開発では危険すぎます。これをセキュアにしようとすると、.netrc を使ったりするややこしい手順が必要でした。

ところが、2013年10月22日にリリースされた Git Plugin 2.0 から、Credential Plugin と連携して暗号化したパスワードを保存できるようになったため、大幅に設定が楽になりました。

以下にそのやり方をメモしておきます。

<!--more-->

リポジトリの URL は `https://foo.com/git/bar.git` とします。

&#8220;Jenkins の管理 > Credentials > Add domain&#8221; をクリックし、Git リポジトリのドメインを入力します。

Specification の URI スキーマには `https` を選択します。

<a href="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_1.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_1.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_1-300x163.png" alt="jenkins_secure_credientials_1" width="300" height="163" class="alignnone size-medium wp-image-1641" /></a>

続いて、&#8221;Add Credentials&#8221; をクリックし、認証情報入力画面を出します。

<a href="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_2.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_2.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_2-300x71.png" alt="jenkins_secure_credientials_2" width="300" height="71" class="alignnone size-medium wp-image-1642" /></a>

&#8220;Kind&#8221; は `ユーザー名とパスワード` を、&#8221;ユーザー名&#8221; には https 認証のユーザー名を入力します。完了したら &#8220;OK&#8221; をクリック。

<a href="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_3.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_3.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_3-300x139.png" alt="jenkins_secure_credientials_3" width="300" height="139" class="alignnone size-medium wp-image-1643" /></a>

これで一旦、認証鍵一覧画面に戻されます。引き続き、いま入力したユーザー名をクリック。

<a href="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_4.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_4.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_4-300x136.png" alt="jenkins_secure_credientials_4" width="300" height="136" class="alignnone size-medium wp-image-1644" /></a>

&#8220;Update&#8221; をクリック。

<a href="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_5.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_5.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_5-300x137.png" alt="jenkins_secure_credientials_5" width="300" height="137" class="alignnone size-medium wp-image-1645" /></a>

&#8220;パスワード&#8221; 欄に認証パスワードを入力し、&#8221;Save&#8221; をクリック。

<a href="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_6.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_6.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_6-300x134.png" alt="jenkins_secure_credientials_6" width="300" height="134" class="alignnone size-medium wp-image-1646" /></a>

以上で foo.com のリポジトリの認証情報が設定できました。

あとはプロジェクトの設定画面を開き、&#8221;ソースコード管理システム > Git&#8221; の &#8220;Repository URL&#8221; に `https://foo.com/git/bar.git` を、&#8221;Credentials&#8221; に先ほど入力した ID/PW のペアを選択すれば OKです。

<a href="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_7.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_7.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_7-300x242.png" alt="jenkins_secure_credientials_7" width="300" height="242" class="alignnone size-medium wp-image-1647" /></a>

これでプロジェクトをビルド可能になります。

## Git 1.7.1 では認証が通らない？

CentOS 6.3 で、EPEL リポジトリから Git をインストールすると 1.7.1 が入りますが、これだとうまく認証が通りませんでした。プロジェクトの設定画面で &#8220;Repository URL&#8221; と &#8220;Credential&#8221; を入力しても、 `Error performing command: ls-remote -h` というエラーが出て進めませんでした。

仕方ないので手動でソースから Git 1.8.4.1 をインストールしたら、無事エラーが出なくなりました。

Git のソースインストール方法手順は以下。いつもの Configure -> make -> make install です。

```
cd /usr/local/src
wget https://git-core.googlecode.com/files/git-1.8.4.1.tar.gz
tar zxvf git-1.8.4.1.tar.gz
cd git-1.8.4.1
./configure
make
make install
```


なお、この場合 Git のバイナリが `/usr/local/bin` 配下にインストールされるので、&#8221;Jenkins の管理 > システム設定 > Git > Path to Git executable&#8221; に `/usr/local/bin/git` と入力する必要があります。

<a href="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_81.png" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_81.png', '']);" ><img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_81-255x300.png" alt="jenkins_secure_credientials_8" width="255" height="300" class="alignnone size-medium wp-image-1649" /></a>

## 参考

  * <a href="https://wiki.jenkins-ci.org/display/JENKINS/Git+Plugin" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://wiki.jenkins-ci.org/display/JENKINS/Git+Plugin', 'Git Plugin &#8211; Jenkins &#8211; Jenkins Wiki']);" >Git Plugin &#8211; Jenkins &#8211; Jenkins Wiki</a>
  * <a href="http://qiita.com/naonya3/items/54c8e3436212ad6686b3" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://qiita.com/naonya3/items/54c8e3436212ad6686b3', 'CentOS6.3にgitをソースコードから入れる &#8211; Qiita [キータ]']);" >CentOS6.3にgitをソースコードから入れる &#8211; Qiita [キータ]</a>