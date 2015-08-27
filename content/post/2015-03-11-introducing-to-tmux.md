---
title: tmuxはじめました
author: 1000k
layout: post
date: 2015-03-11
url: /2015/03/11/introducing-to-tmux/
categories:
  - Linux
  - コマンド
  - 開発環境
tags:
  - Linux
  - 開発環境
---
とあるきっかけで開発効率強化ウィークに入りました。まずはターミナルをもっと効率的にすべく、tmux を習得することにしました。

## tumxとは？

  * ターミナルマルチプレクサ (端末多重化ソフトウェア, 仮想端末マネージャー) のひとつ
      * ほかには GNU screen が有名
  * 読み方は &#8220;ティーマックス&#8221;

### できること

  * 1つの端末で複数の仮想端末を起動できる
  * 仮想端末の画面を自由に分割・統合できる
  * マウスを使わずキーボードだけでコピペできる
  * デタッチ(切り離し)とアタッチ(接続)により、ネットワークが切れても同じセッションを再開できる
  * ステータスバーなどでターミナルの情報をリッチにできる
      * 現在時刻、Wi-Fi接続強度、バッテリー残量なども表示できる
  * 設定ファイルで高度にカスタマイズできる

### tmuxの構成

tmuxはサーバー/クライアント構成を取る。

  * tmuxサーバー(セッション)
      * tmux起動時に生成される
      * セッションを管理するプロセス
  * tmuxクライアント(pty; 仮想端末)
      * tmuxセッションへ接続(アタッチ)しているプロセス(pty)

### 画面の内包関係

`セッション > ウィンドウ > ペイン`

  * セッション: ウィンドウの集まり。
  * ウィンドウ: ペインを管理する領域。タブのイメージ。
  * ペイン: ウィンドウを分割した領域。

## インストール

yum ユーザーは EPEL リポジトリからインストールが可能。

```
sudo yum install -y tmux
```

## チュートリアル

### 起動

`tmux` または `tmux new-session` で新規セッションを起動すると、新しい仮想端末が開く。

### ペインの操作

`C-b %` でペインを分割し、`C-b n` で次のウィンドウに移動します。

`C-b ?` でヘルプが開くので、片方のペインにヘルプを表示した状態で、もう片方でいろいろ実験してみると理解が進みます。

### デタッチ

仮想端末上で `C-b d` で、端末がセッションからデタッチされます。

デタッチ状態ではセッションは残っているので、再度 `tmux attach -t セッション番号` と入力すれば、最後の画面から再開できます。

## 基本操作

### セッションの操作

| operation        | command                                                                            |
| ---------------- | ---------------------------------------------------------------------------------- |
| セッションの作成         | `tmux` or `tmux new-session`                                                       |
| セッションの確認         | `tmux list-sessions`                                                               |
| 接続されているクライアントの確認 | `tmux list-client`                                                                 |
| セッションのアタッチ       | `tmux attach -t セッション番号` or `tmux a` (`-d` を指定すると、そのセッションに接続しているほかのクライアントはデタッチされる) |
| セッションの削除         | `tmux kill-session [-t セッション番号]` (引数を指定しなければ直近のセッションが削除される)                        |
| 全セッションの削除        | `tmux kill-server`                                                                 |

### ウィンドウの操作

| operation       | command                               |
| --------------- | ------------------------------------- |
| ヘルプ表示           | `C-b ?` (閉じるには `q`)                   |
| ウィンドウ作成         | `C-b c`                               |
| ウィンドウ削除         | `C-b &` (ステータスバーに確認が出るので `y` で削除) |
| ウィンドウ名変更        | `C-b ,`                               |
| ウィンドウ一覧表示/移動    | `C-b w` (カーソルキーで選択、[Enter] で表示)       |
| 前のウィンドウに移動      | `C-b p`                               |
| 次のウィンドウに移動      | `C-b n`                               |
| 最後に操作したウィンドウへ移動 | `C-b l`                               |
| 指定したウィンドウへ移動    | `C-b ウィンドウ番号`                         |
| デタッチ            | `C-b d`                               |

### ペインの操作

| operation     | command                                  |
| ------------- | ---------------------------------------- |
| ペイン番号表示       | `C-b q` or `C-b :display-panes`          |
| 指定したペインへ移動    | `C-b q ペイン番号` (インジケーターが表示されている間に番号を入力する) |
| 最後に操作したペインへ移動 | `C-b ;`                                  |
| ペイン分割 (水平方向)  | `C-b "`                                  |
| ペイン分割 (垂直方向)  | `C-b %`                                  |
| ペイン分割解除       | `C-b !`                                  |
| ペイン強制終了       | `C-b x` (ステータスバーに確認が出るので `y` で削除)        |
| ペイン間移動        | `C-b o`                                  |
| ペイン入れ替え       | `C-b {`                                  |

## カスタマイズ

`~/.tmux.conf` にカスタマイズ設定を書き込むことができます。

yum でインストールした場合は `/usr/share/doc/tmux-1.6/examples/` 配下にサンプルファイルがあります。vimmer は <a href="https://github.com/jordansissel/tmux/blob/master/trunk/examples/vim-keys.conf" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'https://github.com/jordansissel/tmux/blob/master/trunk/examples/vim-keys.conf', 'vim-keys.conf']);" >vim-keys.conf</a> を設定するとペイン操作が楽になるかもしれません。

## 参考

  * <a href="http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man1/tmux.1?query=tmux&sec=1" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man1/tmux.1?query=tmux&sec=1', 'OpenBSD manual pages']);" >OpenBSD manual pages</a>
      * 公式マニュアル。英語。
  * <a href="http://kanjuku-tomato.blogspot.jp/2014/02/tmux.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://kanjuku-tomato.blogspot.jp/2014/02/tmux.html', 'tmuxを使い始めたので基本的な機能の使い方とかを整理してみた &#8211; 完熟トマト']);" >tmuxを使い始めたので基本的な機能の使い方とかを整理してみた &#8211; 完熟トマト</a>
      * 概念図やスクリーンショット満載でわかりやすいです。
  * <a href="http://room6933.com/mymemo/tmux/tmux-basic.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://room6933.com/mymemo/tmux/tmux-basic.html', 'tmux基本のコマンド — nato&#8217;s memo 1.0 documentation']);" >tmux基本のコマンド — nato&#8217;s memo 1.0 documentation</a>
  * <a href="http://qiita.com/b4b4r07/items/01359e8a3066d1c37edc" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://qiita.com/b4b4r07/items/01359e8a3066d1c37edc', 'Vim &#8211; ターミナルマルチプレクサ tmux をカスタマイズする &#8211; Qiita']);" >Vim &#8211; ターミナルマルチプレクサ tmux をカスタマイズする &#8211; Qiita</a>
      * 使えるカスタマイズの例。zsh と連携して、ログイン時にいきなり tmux セッションにアタッチする方法など。