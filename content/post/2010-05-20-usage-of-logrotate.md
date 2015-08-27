---
title: '[Linux] logrotateの使い方'
author: 1000k
layout: post
date: 2010-05-20
url: /2010/05/20/usage-of-logrotate/
categories:
  - Linux
tags:
  - Linux
  - logrotate
  - コマンド
---
# 設定チュートリアル

  * &#8220;/etc/logrotate.d/&#8221; 配下に設定ファイルを作成する
  * 「logrotate -d 作成したファイル名」でデバッグする
  * 翌日午前4時にローテーションが成功していることを確認する
      * 実行ログ: /var/lib/logrotate.status

# 設定ファイルの書き方

```
# コメントアウトは「#」で
対象ファイル [,対象ファイル, ...] {
        オプション1
        オプション2
        ...
        オプションN
}
```


## 設定例

  * 対象ファイル:
      * /hige/log/ ディレクトリ内の *.log ファイルすべて
      * /moja/log/ ディレクトリ内の *.log ファイルすべて
  * オプション
      * 毎日実行
      * 対象ファイルが見つからなくてもエラーで停止しない
      * ログファイルが空ならログローテーションしない
      * 7世代前まで管理
      * 1MB以上のログファイルだけログローテーションする

```
/hige/log/*.log /moja/log/*.log {
        daily
        missingok
        notifempty
        rotate 7
        size 1M
}
```


# logrotateの実行方法

基本的には毎朝4:00に自動実行されるが、デバッグと即時ログローテートを行うこともできる。

参考: <a href="http://www.atmarkit.co.jp/flinux/rensai/linuxtips/749rotatetest.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.atmarkit.co.jp/flinux/rensai/linuxtips/749rotatetest.html', '＠IT：logrotateのテストを行うには']);" title="＠IT：logrotateのテストを行うには">＠IT：logrotateのテストを行うには</a>

  * デバッグ

```
logrotate -d /etc/logrotate.d/対象ファイル
```


  * 即時ログローテート

```
logrotate -f 対象ファイル
 logrotate -f /etc/logrotate.conf   # 登録されているローテーションすべて実行
 logrotate -f /etc/logrotate.d/test # testだけ実行
```


# 関連ファイルの場所

  * 設定ファイル
      * /etc/logrotate.d/対象ファイル
  * 実行ログ
      * /var/lib/logrotate.status

# オプション一覧

参考: <a href="http://www.asahi-net.or.jp/~aa4t-nngk/logrotate.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.asahi-net.or.jp/~aa4t-nngk/logrotate.html', 'Stray Penguin &#8211; Linux Memo (logrotate)']);" title="Stray Penguin - Linux Memo (logrotate)">Stray Penguin &#8211; Linux Memo (logrotate)</a>

| 設定値                               | 説明                                                                                                                                                          |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| create \[mode\] \[owner\] [group] | ローテーションを (下記の postrotate も) 行った後、代わりに空の新規ログファイルを作って置いてくる。その属性も指定できる。mode は 0755 のようなオクテット書式。指定しない属性については元のファイルの属性が引き継がれる                                    |
| nocreate                          | 上記をグローバルな設定にした場合に、個別規定内で create を無効にしたい際に使用                                                                                                                 |
| copy/nocopy                       | 元のログファイルはそのままにして、そのコピーを保存する。ログファイルをリロードする術のないプログラムへの対処法のひとつ                                                                                                 |
| copytruncate/nocopytruncate       | copy の動作を行った後、元のログファイルの内容 を消去する。見かけ的には create と同じ結果となる                                                                                                      |
| rotate _num_                      | 世代ローテーションのステップ数。例えば元のログファイルが a.log だとして、 num を 2 にしておくと a.log => a.log.1 => a.log.2 => 廃棄 となる。0 だと a.log => 廃棄                                              |
| start _num_                       | 初代ローテーションファイルの末尾に付加するナンバー。デフォルトは 1。 nuｍ を例えば 5 にすると、a.log => a.log.5 => a.log.6 => &#8230; と推移                                                              |
| extension _ext_                   | ローテーションした旧ログファイルに拡張子 ext を付ける。ドットも必要。例えば some.log を &#8220;extension .bak&#8221; の設定でローテーションすると、初代ローテーションログは some.log.1.bak となる。圧縮も行う場合、圧縮による拡張子はさらにその後ろに付く |
| compress/nocompress               | ローテーションした後の旧ファイルに圧縮を掛ける。デフォルトは nocompress                                                                                                                   |
| compresscmd _cmd_                 | ログファイルの圧縮に使用するプログラムを指定。デフォルトは gzip                                                                                                                          |
| uncompresscmd _cmd_               | ログファイルの解凍に使用するプログラムを指定。デフォルトは gunzip                                                                                                                        |
| compressoptions _opt_             | 圧縮プログラムへ渡すオプション。デフォルトは gzip に渡す &#8220;-9&#8221; (圧縮率最大)。現在のところ &#8220;-9 -s&#8221; のようにスペース入りで複数のオプションを指定することは不可能                                          |
| compressext _ext_                 | 圧縮後のファイルに付ける拡張子 (ドットも必要)。デフォルトでは、使用する圧縮コマンドに応じたものが付くとされているが、bzip2 を使っても gz になってしまうなど、あまり当てにならない                                                             |
| delaycompress/nodelaycompress     | 圧縮処理を、その次のローテーションの時まで遅らせる。compress も指定しないと無意味                                                                                                               |
| olddir _dir_                      | ローテーションした旧ログを dir に移動する。移動先は元と同じデバイス上でなければならない。元のログに対する相対指定も有効                                                                                              |
| noolddir                          | 上記の否定                                                                                                                                                       |
| mail address                      | 旧ログファイルを address に送信する。どの段階のものを送るかは maillast などのオプションで決まる                                                                                                   |
| nomail                            | 上記の否定                                                                                                                                                       |
| maillast                          | mail に関するオプション。最終世代を経て破棄されるログをメールする (デフォルト)                                                                                                                 |
| mailfirst                         | mail に関するオプション。初代ローテーションログをメールする                                                                                                                            |
| daily/weekly/monthly              | ログローテーションを日毎/週毎/月毎に行う。デフォルトは daily。例えば weekly なら、毎日実行を掛けたとしても、その週で最初に必要条件を満たした際にだけローテーションが行われる                                                              |
| size _num[K/M]_                   | ログのサイズが num バイトを超えていればローテーションを行う。この条件は daily, weekly などの条件より優先される。キロ/メガバイトでの指定も可能                                                                           |
| ifempty                           | 元のログファイルが空でもローテーションを行う (デフォルト)                                                                                                                              |
| notifempty                        | 元のログファイルが空ならばローテーションしない                                                                                                                                     |
| missingok                         | 指定のログファイルが実在しなかったとしてもエラーを出さずに処理続行                                                                                                                           |
| nomissingok                       | 指定のログファイルが実在しなければエラーを出力 (デフォルト)                                                                                                                             |
| firstaction _script_ endscript    | 実際にローテーションの条件に合致するログファイルがひとつでもあった場合に、ローテーションを行う前 (prerotate のアクションよりも前) に一度だけ実行するスクリプト。個別定義内でのみ指定可能                                                         |
| prerotate _script_ endscript      | 実際にローテーションの条件に合致するログファイルがひとつでもあった場合に、ローテーションの前に (firstaction よりは後) に実行するスクリプト。個別定義内でのみ指定可能                                                                  |
| postrotate _script_ endscript     | 実際にローテーションが行われた後 (lastaction よりは前) に実行するスクリプト。個別指定内でのみ指定可能                                                                                                  |
| lastaction _script_ endscript     | 実際にローテーションが行われた後 (postrotate よりも後) に一度だけ実行するスクリプト。個別指定内でのみ指定可能                                                                                              |
| nosharedscripts                   | ローテーションの条件に合致するログが複数あった場合に、prerotate, postrotate のスクリプトを各ログファイル毎に実行する (デフォルト)                                                                               |
| sharedscripts                     | ローテーションの条件に合致するログが複数あった場合に、prerotate, postrotate のスクリプトを一度だけ実行する                                                                                            |
| include file\_or\_dir             | 設定ファイル内でこの記述のある位置に別の設定ファイルを読み込む。ディレクトリを指定した場合、その dir 内から、ディレクトリおよび名前付きパイプ以外のレギュラーファイルがアルファベット順に読み込まれる                                                       |
| tabooext _[+] ext[,ext,&#8230;]_  | include でディレクトリを指定した場合に読み込みから除外するファイルの拡張子を指定。デフォルトで .rpmorig, .rpmsave, .rpmnew, .v, .swp, ~ が設定されている。+ を挟むと追加指定、挟まないと根こそぎ置き換えとなる                           |

# copytruncate方式の欠点

参考: <a href="http://kamoland.com/wiki/wiki.cgi?logrotate%A4%CE%C0%DF%C4%EA" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://kamoland.com/wiki/wiki.cgi?logrotate%A4%CE%C0%DF%C4%EA', 'logrotateの設定 &#8211; KamoLand']);" title="logrotateの設定 - KamoLand">logrotateの設定 &#8211; KamoLand</a>

> 今回の方式ではcopytruncateを使っているため，ローテーション時には
>
>   1. 元ファイルのコピーを作り
>   2. 元ファイルの内容をクリアする
>
> という動作によってローテーションを実現している．
>
> なので，ログを出力しているプロセス(Apache)から見れば永遠に同じファイルに出力し続ければよいため，Apacheにファイルの切り替えを認識させるために再起動をかける必要もない．
>
> しかし見てわかるように，1.と2.の間のわずかな時間に発生したログは，
>
>   * コピーのファイルには含まれず
>   * しかもクリアされる
>
> ということになってしまい，結果として消滅してしまう．これがcopytruncate方式の欠点．
>
> もしこのようなロストが許されない場合はcopytruncate方式は使えないので，
>
>   * copytruncateを使わずに，かつpostrotateでApacheの再起動を行う
>   * Apacheでログをパイプに出力するように設定して，パイプの内容をrotatelogs，cronologなどでファイルに仕分けする
>
> などの方法を採る必要があります．