---
title: Modelのbake中に出る「A displayField could not be automatically detected」とは？
author: 1000k
layout: post
date: 2010-07-06
url: /2010/07/06/what-is-a-displayfield-could-not-be-automatically-detected/
categories:
  - CakePHP
  - PHP
tags:
  - CakePHP
---
Modelをbakeしていると、「A displayField could not be automatically detected would you like to choose one? (y/n)」というメッセージが出ることがあります。

displayFieldというのは、CakePHPが自動で「これは画面に表示するフィールドだろう」と認識してくれるフィールド。具体的には「name」「title」などが該当します。

今bakeしようとしているテーブルにそうした名前のフィールドが見つからない場合、自分で指定しなければならない、ということのようです。

自動で認識されるフィールド名と、それがどう利用されるかの関係は[Cookbook](http://book.cakephp.org/ja/view/1014/Titles)に書いてありました。

### 参考

[Nabble - CakePHP - Cake Console Baking Model displayField?](http://cakephp.19694.n2.nabble.com/Cake-Console-Baking-Model-displayField-td4976131.html)