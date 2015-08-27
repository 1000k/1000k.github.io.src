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

"Jenkins の管理 > Credentials > Add domain" をクリックし、Git リポジトリのドメインを入力します。

Specification の URI スキーマには `https` を選択します。

[<img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_1-300x163.png" alt="jenkins_secure_credientials_1" width="300" height="163" class="alignnone size-medium wp-image-1641" />](http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_1.png)

続いて、"Add Credentials" をクリックし、認証情報入力画面を出します。

[<img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_2-300x71.png" alt="jenkins_secure_credientials_2" width="300" height="71" class="alignnone size-medium wp-image-1642" />](http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_2.png)

"Kind" は `ユーザー名とパスワード` を、"ユーザー名" には https 認証のユーザー名を入力します。完了したら "OK" をクリック。

[<img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_3-300x139.png" alt="jenkins_secure_credientials_3" width="300" height="139" class="alignnone size-medium wp-image-1643" />](http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_3.png)

これで一旦、認証鍵一覧画面に戻されます。引き続き、いま入力したユーザー名をクリック。

[<img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_4-300x136.png" alt="jenkins_secure_credientials_4" width="300" height="136" class="alignnone size-medium wp-image-1644" />](http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_4.png)

"Update" をクリック。

[<img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_5-300x137.png" alt="jenkins_secure_credientials_5" width="300" height="137" class="alignnone size-medium wp-image-1645" />](http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_5.png)

"パスワード" 欄に認証パスワードを入力し、"Save" をクリック。

[<img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_6-300x134.png" alt="jenkins_secure_credientials_6" width="300" height="134" class="alignnone size-medium wp-image-1646" />](http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_6.png)

以上で foo.com のリポジトリの認証情報が設定できました。

あとはプロジェクトの設定画面を開き、"ソースコード管理システム > Git" の "Repository URL" に `https://foo.com/git/bar.git` を、"Credentials" に先ほど入力した ID/PW のペアを選択すれば OKです。

[<img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_7-300x242.png" alt="jenkins_secure_credientials_7" width="300" height="242" class="alignnone size-medium wp-image-1647" />](http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_7.png)

これでプロジェクトをビルド可能になります。

## Git 1.7.1 では認証が通らない？

CentOS 6.3 で、EPEL リポジトリから Git をインストールすると 1.7.1 が入りますが、これだとうまく認証が通りませんでした。プロジェクトの設定画面で "Repository URL" と "Credential" を入力しても、 `Error performing command: ls-remote -h` というエラーが出て進めませんでした。

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


なお、この場合 Git のバイナリが `/usr/local/bin` 配下にインストールされるので、"Jenkins の管理 > システム設定 > Git > Path to Git executable" に `/usr/local/bin/git` と入力する必要があります。

[<img src="http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_81-255x300.png" alt="jenkins_secure_credientials_8" width="255" height="300" class="alignnone size-medium wp-image-1649" />](http://blog.1000k.net/wp-content/uploads/jenkins_secure_credientials_81.png)

## 参考

  * [Git Plugin - Jenkins - Jenkins Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Git+Plugin)
  * [CentOS6.3にgitをソースコードから入れる - Qiita [キータ]](http://qiita.com/naonya3/items/54c8e3436212ad6686b3)