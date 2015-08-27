---
title: git clone した後 git sudmobule init が失敗するときの対処方法
author: 1000k
layout: post
date: 2013-03-27
url: /2013/03/27/what-to-do-if-git-sudmobule-init-fails-after-git-clone/
categories:
  - その他
  - 開発環境
tags:
  - git
  - トラブルシューティング
---
submodule を含むリポジトリを clone した直後は、ディレクトリは存在しますが実ファイルが存在しません。以下のコマンドで初期化する必要があります。

```
$ git clone git://mojamoja/uso.git
$ cd uso/
$ git submodule init
$ git submodule update
```


これで実ファイルがローカルにダウンロードされます。

…らしいですが、うまくいきませんでした。

私の場合、"git submodule init" を叩いたら以下のエラーが出ました。

```
No submodule mapping found in .gitmodules for path 'chef-repo/cookbooks/ant'
```


原因はプロジェクト内の .gitmodules にありました。

Windows で "git submodule add" した時に一部の path が "&#92;" という表記になっており、これを git がうまく解釈できなかった模様。

```
[submodule "chef-repo\\cookbooks\\java"]
     path = chef-repo\\cookbooks\\java
     url = git://github.com/opscode-cookbooks/java.git
```


"&#92;" を "/" に置換してやればOKです。```
 [submodule "chef-repo/cookbooks/ava"] path = chef-repo/cookbooks/java url = git://github.com/opscode-cookbooks/java.git ```


これで "git sudmoule init" が成功するようになりました。

## 参考

  * [Git clone vs Git submodule : Ed Spencer](http://edspencer.net/2008/04/git-clone-vs-git-submodule.html)