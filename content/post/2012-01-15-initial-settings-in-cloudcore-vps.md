---
title: CloudCore VPS初期設定
author: 1000k
layout: post
date: 2012-01-15
url: /2012/01/15/initial-settings-in-cloudcore-vps/
categories:
  - Linux
  - その他
  - 開発環境
tags:
  - CentOS
  - CloudCore
  - FTP
  - Linux
  - VPS
  - yum
  - 設定
---
高スペック＆安価＆国産で話題のCloudCore VPSをレンタルしてみました。

ざっとスペックを確認すると、以下のようになりました。これで月980円はたしかにお得です。

```
# cat /etc/redhat-release
CentOS release 5.6 (Final)

# df -h
Filesystem          サイズ  使用  残り 使用% マウント位置
/dev/vda1              97G  5.0G   87G   6% /
tmpfs                1003M     0 1003M   0% /dev/shm

# cat /proc/cpuinfo
processor       : 0
vendor_id       : AuthenticAMD
cpu family      : 16
model           : 2
model name      : AMD Phenom(tm) 9550 Quad-Core Processor
stepping        : 3
cpu MHz         : 2199.998
cache size      : 512 KB
fpu             : yes
fpu_exception   : yes
cpuid level     : 5
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 syscall nx mmxext fxsr_opt pdpe1gb lm up pni cx16 popcnt lahf_lm cmp_legacy svm cr8_legacy altmovcr8 abm sse4a misalignsse
bogomips        : 4399.99
TLB size        : 1024 4K pages
clflush size    : 64
cache_alignment : 64
address sizes   : 40 bits physical, 48 bits virtual
power management:

# cat /proc/meminfo
MemTotal:      2053764 kB
MemFree:       1859380 kB
```


今後利用するかもしれないので、初期設定メモを残しておきます。

<!--more-->

# よく使うパッケージのインストール

CentOS最小構成でインストールされているらしく、よく使うパッケージが全然入っていません。入れておきましょう。

```
# yum -y install sudo vim-enhanced iptables vsftpd bzip2 gcc gcc-c++ make automake mlocate
```


導入しているパッケージの説明は下記の通り。

| パッケージ名         | 説明                     |
| -------------- | ---------------------- |
| sudo           | sudoコマンド               |
| vim-enhanced   | 標準で入っているviの高機能版        |
| iptables       | ファイアーウォール              |
| vsftpd         | FTPデーモン                |
| bzip2          | bzip2圧縮・解凍             |
| gcc, gcc-c++   | コンパイル時に必要              |
| make, automake | ソースからインストールする時必要       |
| mlocate        | locate & updatedbコマンド  |
| vixie-cron     | crontabコマンド & cronデーモン |

crondは起動し、自動起動をオンにしておきます。

```
# /etc/rc.d/init.d/crond start
# chkconfig crond on
```


また、インストール済みのパッケージを更新しましょう。

私の場合100パッケージ(129MB)ありました。

```
# yum update
```


# 作業用ユーザー追加

rootでの作業はリスク上よろしくないので、作業用ユーザーを追加します。

wheelグループに追加することで、su権限が得られます。

```
# useradd mojamoja
# usermod -G wheel mojamoja
```


wheelグループのユーザーがsudoを実行できるよう設定します。

```
# visudo

----
## Allows people in group wheel to run all commands
%wheel  ALL=(ALL)       ALL
↑この行のコメントアウトを外す。
```


# sshポートの変更

sshポートデフォルトの22番ではポートスキャンの標的になりやすいので、変更します。

```
#vim /etc/ssh/sshd_config

以下の行を変更。

#port 22
port 10022
```


sshデーモンを再起動して変更を反映します。

これで22番ポートではsshログインできなくなっています。

```
# /etc/init.d/sshd restart
```


# 不要なデーモンの停止

初期状態では4つのデーモンしか自動起動設定になっておらず、特に停止するようなデーモンはありません。

参考: [スペック｜CloudCore VPS｜KDDIウェブコミュニケーションズ](http://www.cloudcore.jp/vps/spec/)

# ファイアーウォールの構築

ssh(10022), FTP, HTTPで接続できるようポートを開放してやります。

下記シェルファイルを作成し、実行してください。

※コンソールから1行ずつ打つと、「/sbin/iptables -P INPUT DROP」設定直後にコンソールが操作不能になります。

その場合、コントロールパネルから再起動することで再度ログイン可能になります。

```
#!/bin/sh

/sbin/iptables -F
/sbin/iptables -X

/sbin/iptables -P INPUT DROP
/sbin/iptables -P OUTPUT ACCEPT
/sbin/iptables -P FORWARD DROP

/sbin/iptables -A INPUT -i lo -j ACCEPT
/sbin/iptables -A OUTPUT -o lo -j ACCEPT

/sbin/iptables -A INPUT -s 10.0.0.0/8 -j DROP
/sbin/iptables -A INPUT -s 172.16.0.0/12 -j DROP
/sbin/iptables -A INPUT -s 192.168.0.0/16 -j DROP

/sbin/iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT

/sbin/iptables -A INPUT -p tcp --dport 10022 -j ACCEPT
/sbin/iptables -A INPUT -p tcp --dport 80 -j ACCEPT

/sbin/iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

/etc/rc.d/init.d/iptables save

/sbin/service iptables restart
```


設定が終わったら実行します。

```
# chmod +x set_iptables.sh
# ./set_iptables.sh
ファイアウォールのルールを /etc/sysconfig/iptables に保存中[  OK  ]
ファイアウォールルールを適用中:                            [  OK  ]
チェインポリシーを ACCEPT に設定中filter                   [  OK  ]
iptables モジュールを取り外し中                            [  OK  ]
iptables ファイアウォールルールを適用中:                   [  OK  ]
```


無事設定できたら、サーバー起動時にサービスが開始するよう設定します。設定が間違ったままこれをonにすると、コントロールパネルから再起動してもログイン不能になるので注意してください。

```
# chkconfig iptables on
```


# FTPを接続可能にする

**/etc/vsftpd/vsftpd.conf** を以下のように設定します。

```
以下の既存の行を修正する。

# 匿名ユーザーのログインを禁止
anonymous_enable=NO

# asciiモードでファイルを転送可能にする
ascii_upload_enable=YES
ascii_download_enable=YES

以下の行を追加。

# ファイル所有者を数字ではなくユーザー名で表示する
text_userdb_names=YES

# ファイルの上書き時間が日本時間になる
use_localtime=YES
```


設定が終わったら起動し、自動起動もオンにしましょう。

```
# /etc/rc.d/init.d/vsftpd start
vsftpd 用の vsftpd を起動中:                               [  OK  ]
# chkconfig vsftpd on
```


FTPクライアントで10022番ポートにSFTP接続可能になっていることを確認してください。

# 参考

  * [CentOSをサーバーとして活用するための基本的な設定 - さくらインターネット創業日記](http://tanaka.sakura.ad.jp/archives/001065.html)
      * さくらの社長による、自社のVPSの初期設定方法
  * [CentOS:sudo を設定する - 日々のメモ](http://d.hatena.ne.jp/a__z/20071011)
  * [はじめてのさくら VPS + CentOS の初期設定からチューニングなどの作業まとめ | ウェブル](http://weble.org/2011/05/16/sakura-vps-and-centos)
      * LAMP環境の構築手順も
  * [さくらのVPS を使いはじめる 3 – iptables を設定する | アカベコマイリ](http://akabeko.sakura.ne.jp/blog/2010/09/%E3%81%95%E3%81%8F%E3%82%89%E3%81%AEvps-%E3%82%92%E4%BD%BF%E3%81%84%E3%81%AF%E3%81%98%E3%82%81%E3%82%8B-3/)
  * [クライアントＰＣから、ＬINUXサーバー(Fedora9)にFTPで接続する。](http://www.searchman.info/linux/1070.html)
      * vsftpdの設定方法
  * [評判のCloudCore VPSを使うときに最初にやっておきたいこと(CentOS編) | レンタルサーバー・自宅サーバー設定・構築のヒント](http://sakura.off-soft.net/blog/cloudcore_vps_centos_first_setup.html)
  * [スペック｜CloudCore VPS｜KDDIウェブコミュニケーションズ](http://www.cloudcore.jp/vps/spec/)