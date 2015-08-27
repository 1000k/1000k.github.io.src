---
title: Jenkins が古い Java JDK を参照するせいで起動しなくなった
author: 1000k
layout: post
date: 2013-02-19
url: /2013/02/19/jenkins-cannot-run-because-of-refering-old-java-jdk/
categories:
  - CI
  - Jenkins
tags:
  - CI
  - Jenkins
  - テスト
  - トラブルシューティング
---
CentOS 5.2 で発生した不具合。Jenkins を最新版 (1.502)にアップデートしたら起動できなくなりました。

```
# service jenkins start
Starting Jenkins Jenkins requires Java5 or later, but you are running 1.4.2 from /usr/lib/jvm/java-1.4.2-gcj-1.4.2.0/jre
java.lang.UnsupportedClassVersionError: 48.0
   at Main.main(Main.java:90)
                                                           [  OK  ]
(OK と出るが起動せず)
```


JDK 1.6.0 がインストールされているにも関わらず、なぜか CentOS デフォルトの 1.4.2 を見に行っています。

修正方法をメモしておきます。

<!--more-->

OS がどの JDK を参照しているのかは、 <span class="lang:default decode:true crayon-inline">update-alternatives -display java</span> コマンドで確認することができます。

叩いてみると、やはり古い JDK を参照していました。

```
# update-alternatives --display java

java -ステータスは自動です。
リンクは現在 /usr/lib/jvm/jre-1.4.2-gcj/bin/java を指しています。
/usr/lib/jvm/jre-1.4.2-gcj/bin/java - 優先項目 1420
 スレーブ jre: /usr/lib/jvm/jre-1.4.2-gcj
 スレーブ jre_exports: /usr/lib/jvm-exports/jre-1.4.2-gcj
 スレーブ keytool: /usr/lib/jvm/jre-1.4.2-gcj/bin/keytool
 スレーブ rmiregistry: /usr/lib/jvm/jre-1.4.2-gcj/bin/rmiregistry
現在の「最適」バージョンは /usr/lib/jvm/jre-1.4.2-gcj/bin/java です。
```


これを最新版に切り替えたいのですが、私は手動で JDK 1.6.0 をインストールしたせいで、update-alternatives に認識されていませんでした。

<span class="lang:default decode:true crayon-inline">-config java</span> オプションで確認してみると、JDK 1.4.2 しか表示されません。

```
# update-alternatives --config java

1 プログラムがあり 'java' を提供します。

  選択       コマンド
-----------------------------------------------
*+ 1           /usr/lib/jvm/jre-1.4.2-gcj/bin/java

Enter を押して現在の選択 [+] を保持するか、選択番号を入力します:
```


というわけで、JDK 1.6.0_32 のパスを追加して、リンクを書きなおしてやります。

以下のコマンドでリンクを追加することができます。

```
# update-alternatives --install /usr/bin/java java /usr/java/jdk1.6.0_32/bin/java 16032
```


これでOKです。

再び <span class="lang:default decode:true crayon-inline">update-alternatives -config java</span> コマンドでリンクの書き換えが成功してしていることを確かめられます。

```
# update-alternatives --config java

2 プログラムがあり 'java' を提供します。

  選択       コマンド
-----------------------------------------------
   1           /usr/lib/jvm/jre-1.4.2-gcj/bin/java
*+ 2           /usr/java/jdk1.6.0_32/bin/java
```


以上で Jenkins が起動するようになりました。

```
# service jenkins start
Starting Jenkins                                           [  OK  ]
```


## 参考

  * [How do I configure starting jdk for hudson(now jenkins)? - Stack Overflow](http://stackoverflow.com/questions/9527737/how-do-i-configure-starting-jdk-for-hudsonnow-jenkins)
  * [update-alternativesで複数のバージョンを持つプログラムの切り替え | kyamamoto at blog.neofig.com](http://blog.neofig.com/2011/03/11/update-alternatives%E3%81%A7%E8%A4%87%E6%95%B0%E3%81%AE%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E3%82%92%E6%8C%81%E3%81%A4%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%A0%E3%81%AE%E5%88%87%E3%82%8A/)
  * [update-alternativesのグループにインストールしたJavaが追加されていないのかも](http://piggydb.jp/document-view.htm?id=597)