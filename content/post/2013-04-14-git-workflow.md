---
title: git ワークフロー
author: 1000k
layout: post
date: 2013-04-14
url: /2013/04/14/git-workflow/
categories:
  - その他
  - 開発環境
tags:
  - git
  - チュートリアル
---
実際のプロジェクトを git で運用する時のよくある流れをまとめました。

<!--more-->

## 前準備

### git push コマンドのデフォルトの挙動を変更する

git push コマンドのデフォルトの挙動では、引数を指定しないとローカルにある全てのブランチを push してしまいます。そのため、リモートとローカルに同名のブランチがある場合、リモートブランチを同名のローカルブランチで意図せず上書きしてしまう危険があります。

そこで、引数なしの git push を実行した時は、現在編集中のブランチのみ push するようにします。

```
$ git config  --global push.default current
```


## 新しいブランチを作成して作業する

以下の流れを行う時のコマンドです。

  1. mojamoja リポジトリに新しいブランチを作成する
  2. 変更をリモートに push する
  3. 変更点を master ブランチにマージする
  4. 変更したバージョンをタグにする

```
# mojamoja リポジトリをローカルに clone する
$ git clone git@bitbucket.org:1000k/mojamoja
$ cd mojamoja

# 現在ローカルにあるブランチを確認する
$ git branch -l

# 作業用ブランチ (例として 'feature-branch' という名前) を作成する
$ git branch feature-branch

# 作業用ブランチに切り替える
$ git checkout feature-branch

# 編集
$ vim ...
$ vim ...

# 変更のあったファイルを確認する
$ git status

# 変更したファイルを全てコミット対象に入れる
# 個別に登録したい場合は "git add {ファイルパス}"
$ git add -A

$ git commit -m "Foo クラスを作成。"

# 変更したファイルをリモートにプッシュする
$ git push

# master ブランチにマージする
$ git checkout master
$ git merge feature-branch
$ git push

# 既存のタグを確認
$ git tag -l

# ローカルにタグを作成する
$ git tag 0.0.1 -m "First tag"

# リモートに push する
$ git push --tags

# 使わなくなったローカルブランチを削除する
$ git branch -d feature-branch

# 使わなくなったリモートブランチを削除する
# ブランチ名の前に ":" を付け忘れると動かないので注意
$ git push origin :feature-branch
```


## ローカルをリモートの状態に合わせる

異なるマシンから同じリモートブランチに変更を加えると、それぞれのマシン間で差異が出てしまいます。以下の手順で、ローカルのブランチをリモートにあるブランチと同期させることが可能です。

```
# リモートの最新のデータを取得する
$ git fetch origin master

# ローカルのバージョンの向き先を最新に変える
$ git reset --hard FETCH_HEAD

# ローカルの管理外ファイルを消す
$ git clean -df
```


これでリモートとローカルでファイルの中身が同一になりました。

## リモートブランチをローカルで編集する

リモートにのみ 0.2.0 ブランチがあり、ローカルにはまだ無い場合、以下のコマンドでローカルにコピーすることができます。

```
# ローカルにあるブランチの一覧を確認する
$ git branch -r

# ローカルの 0.2.0 ブランチにリモートの 0.2.0 をチェックアウトする
$ git checkout -b 0.2.0 origin/0.2.0
```


## 間違ったコミットを取り消す

2つの考え方があるので注意。

ここは git の内部構造を理解していないと飲み込めないかもしれません。

[いつやるの？Git入門](http://www.slideshare.net/matsukaz/git-17499005) が簡潔に内部構造を説明しているので、目を通すことをオススメします。

### commit -amend

後から前回のコミットに追加して変更をするやり方です。

```
# わざと間違ったコミットをする
$ git commit -m "Failure commit"

# これまでのコミットの一覧を確認する
$ git log

# さっきコミットし忘れたファイルをステージングする
$ git add foo.php

# 先ほどコミットした部分と合わせて新たなコミットを行う
git commit --amend -m "Successful commit"
```


これにより、1回目の間違ったコミットは無効化され、2回目のコミットが利用されます。

### git reset

前回のコミットそのものを取り消す (無かったことにする) コマンドです。

コミットしたファイルを残すか消すかで、オプションの値を変える必要があります。

コミットだけを取り消して、変更したファイルはそのままで、1つ手前の状態に戻すには、`--soft` オプションを付けます。

```
git reset --soft HEAD^
```


コミットを取り消し、ワークディレクトリの中身も1つ前の状態に置き換える (= 変更も全て元に戻る) には、`--hard` オプションを付けます。

```
git reset --hard HEAD^
```


## 参考

  * [gitのリモートブランチを使って作業を行う流れのメモ - 那由多屋 開発日誌](http://d.hatena.ne.jp/nayutaya/20090519/1242701594)
  * [git push current branch - Stack Overflow](http://stackoverflow.com/questions/948354/git-push-current-branch)
  * [transitive.info - git tag 使い方](http://transitive.info/article/git/command/tag/)
  * [fetch と pullの違い #git - Qiita](http://qiita.com/items/e082d64f3f8b424e9b7d)
  * [Git pullを使うべきでない３つの理由 - DQNEO起業日記](http://dqn.sakusakutto.jp/2012/11/git_pull.html)
  * [git commitをやり直しする＆取り消しする(「get commit -amend」と「git reset」) - hogehoge foobar Blog Style5](http://d.hatena.ne.jp/mrgoofy33/20100910/1284069468)
  * [いつやるの？Git入門](http://www.slideshare.net/matsukaz/git-17499005)