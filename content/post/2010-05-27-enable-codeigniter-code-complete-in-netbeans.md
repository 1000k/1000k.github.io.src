---
title: '[NetBeans] CodeIgniterの単語補完ができるようにする'
author: 1000k
layout: post
date: 2010-05-27
url: /2010/05/27/enable-codeigniter-code-complete-in-netbeans/
categories:
  - CodeIgniter
  - PHP
  - 開発環境
tags:
  - CodeIgniter
  - NetBeans
  - お役立ち
---
CodeIgniterには便利なライブラリやヘルパが多数用意されてますが、すべて覚えるのは面倒ですし、間違いもおきます。そういったことはIDEに助けてもらいたい。が、私がよく使うNetBeansは、デフォルトでは単語を補完してくれない。

そこで、以下の方法で単語補完をできるようにしました。

### 手順

CodeIgniterをNetBeansのプロジェクトに登録すると、以下のようなディレクトリ構成になっているはず。

```
/application
/error
/images
/nbproject
/scripts
/styles
index.php
.htaccess
```


この「/nbproject」ディレクトリ内に、以下の「netbeans\_ci\_code_completion.php」というファイルを配置するだけです。

```
<?
/**
* @property CI_Loader $load
* @property CI_Form_validation $form_validation
* @property CI_Input $input
* @property CI_Email $email
* @property CI_DB_active_record $db
* @property CI_DB_forge $dbforge
* @property CI_Table $table
* @property CI_Session $session
* @property CI_FTP $ftp
* ....
*/
Class Controller {
}
?>
```


これで、NetBeansが自動補完してくれるようになります。

※上記ファイルだけでは全ライブラリを網羅していません。使いたければ「/system/library」内にあるクラスも同様のフォーマットで追加してあげてください。（時間があったらあとで追記します）

### 参考

<a href="http://www.mybelovedphp.com/2009/01/27/netbeans-revisited-code-completion-for-code-igniter-ii/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.mybelovedphp.com/2009/01/27/netbeans-revisited-code-completion-for-code-igniter-ii/', 'My Beloved PHP » Blog Archive » Netbeans revisited: Code Completion for Code-igniter II']);" title="My Beloved PHP » Blog Archive » Netbeans revisited: Code Completion for Code-igniter II">My Beloved PHP » Blog Archive » Netbeans revisited: Code Completion for Code-igniter II</a>