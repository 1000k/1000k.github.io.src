---
title: Sublime Text 2 で CakePHP2 の CodeSniffer ルールを適用する
author: 1000k
layout: post
date: 2015-01-27
url: /2015/01/27/validate-cakephp2-rules-with-sublime-text2-phpcs-plugin/
categories:
  - CakePHP
  - CI
  - PHP
tags:
  - CakePHP
  - PHP
  - Sublime Text
  - チュートリアル
---
Sublime Text 2 のエディタ上で PHP CodeSniffer を使い、コーディング規約をチェックする方法です。[sublime-phpcs](http://benmatselby.github.io/sublime-phpcs/) プラグインを導入することで、コーディング規約違反のある行がエディタ上に表示されるようになります。

なお、プラグインの公式ページでは phpmd も一緒に導入していますが、[PHPMD はヒステリックすぎて個人的にお勧めしない](http://blog.1000k.net/2013/11/26/phpmd-%e3%81%ae%e9%a0%ad%e3%81%8c%e3%81%8b%e3%81%aa%e3%82%8a-messy/) ので省略します。

<!--more-->

## チュートリアル

### CodeSniffer をインストールする

```
pear install PHP_CodeSniffer
```

### CakePHP のルールセットをインストールする

```
pear channel-discover pear.cakephp.org
pear install cakephp/CakePHP_CodeSniffer
```

CodeSniffer のルールセットに追加されているか以下のコマンドで確認。

```
$ phpcs -i
The installed coding standards are CakePHP, MySource, PEAR, PHPCS, PSR1, PSR2, Squiz and Zend
(CakePHP が含まれていれば OK)
```

### Sublime Text 2 に CodeSniffer プラグインをインストールする

"Package Control: Install Package" で `Phpcs` をインストールするだけ。

(Package Control 自体のインストール方法は [Installation - Package Control](https://sublime.wbond.net/installation#st2) を参照)

### Sublime Text の設定

"Preferences > Package Settings > PHP Code Sniffer > Settings - User" を開き、以下のキーを設定する必要があります。

  * phpcs\_php\_path
  * phpcs\_executable\_path
  * php\_cs\_fixer\_executable\_path

記入例:

```
{
    "phpcs_php_path": "C:\xampp\php\phpcs",
    "phpcs_executable_path": "C:\xampp\php\phpcs.bat",
    "phpcs_additional_args": {
        "--standard": "CakePHP"
    },
    "php_cs_fixer_executable_path": "C:Users{ユーザー名}AppDataRoamingComposervendorbin"
}
```

### (Windows のみ) phpcs.py を修正する

Windows では、このまま phpcs が実行しても何も結果が表示されません。

(`Ctrl + @` でコンソールを開くと `[Windows Error]` と出て途中で止まっている)

以下の手順でプラグインのコードを修正してください。

  1. "Preferences > Browse Packages…" でパッケージディレクトリを開く。
  2. "Phpcs > phpcs.py" を開く。
  3. 169行目を以下のように修正 (パラメータに `shell=True` を追加) する。

```
# proc = subprocess.Popen(cmd, stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.STDOUT, startupinfo=info, cwd=home)
proc = subprocess.Popen(cmd, stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.STDOUT, startupinfo=info, cwd=home, shell=True)
```

### プラグインを実行する

PHP ファイルを開いたタブで `Ctrl + P > PHP Code Sniffer: Sniff This File` とタイプすれば、コーディング規約違反を検知してくれます。

これでコーディングルール違反でチームメイトにディスられるリスクを回避できるようになりました。

## オプション

### 保存時に Sniff されるのを止める

プラグインインストール後のデフォルトだと、保存するたびに規約違反がサジェストされます。

鬱陶しい場合は "Preferences > Package Settings > PHP Code Sniffer > Settings - User" に下記を追加することでオフにできます。

```json
{
  "phpcs_execute_on_save": false
}
```

## 参考

  * [sublime-phpcs](http://benmatselby.github.io/sublime-phpcs/)
  * [php - Custom ruleset for phpcs using PHPStorm - Stack Overflow](http://stackoverflow.com/questions/10585274/custom-ruleset-for-phpcs-using-phpstorm)
  * [cakephp/cakephp-codesniffer · GitHub](https://github.com/cakephp/cakephp-codesniffer)
  * [fabpot/PHP-CS-Fixer · GitHub](https://github.com/fabpot/PHP-CS-Fixer)
  * [sublime-phpcsでPSR-2準拠のコーディング - だいたいよくわからないブログ( ´_ゝ`)](http://matsu-chara.hatenablog.com/entry/2013/12/27/125026)