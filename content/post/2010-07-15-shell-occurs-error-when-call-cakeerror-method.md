---
title: シェルからcakeErrorメソッドを呼び出すと失敗する
author: 1000k
layout: post
date: 2010-07-15
url: /2010/07/15/shell-occurs-error-when-call-cakeerror-method/
categories:
  - CakePHP
  - PHP
tags:
  - CakePHP
  - トラブルシューティング
---
CakePHP 1.3.2 & PHP 5.3.1にて。

CakePHP のエラー処理は「$this->cakeError(<string errorType>, [array parameters]);」を使うといいらしいので、使ってみました。その結果、Controller で使う場合は問題ありませんが、Shell 上で呼び出すとエラーが出てしまいました。

以下、その原因と対策をメモします。

<!--more-->

cakeError() を含む uso シェルを実行した結果は下記。エラー画出ています。

```
../cake/console/cake uso

Notice: Undefined variable: name in C:\xampp\htdocs\sandbox\cake\console\error.php on line 68

Call Stack:
    0.0898     540608   1. {main}() C:\xampp\htdocs\sandbox\cake\console\cake.php:0
    0.1060     541392   2. ShellDispatcher->ShellDispatcher() C:\xampp\htdocs\sandbox\cake\console\cake.php:660
    0.4175    2728408   3. ShellDispatcher->dispatch() C:\xampp\htdocs\sandbox\cake\console\cake.php:139
    1.2805    9165232   4. usoShell->main() C:\xampp\htdocs\sandbox\cake\console\cake.php:377
    1.2805    9165232   5. usoCommonComponent->uho() C:\xampp\htdocs\sandbox\uso\vendors\shells\uso.php:30
    1.2805    9165736   6. Object->cakeError() C:\xampp\htdocs\sandbox\uso\controllers\components\uso_common.php:10
    1.2807    9165992   7. ErrorHandler->__construct() C:\xampp\htdocs\sandbox\cake\libs\object.php:201
    1.3140    9166968   8. call_user_func() C:\xampp\htdocs\sandbox\cake\console\error.php:56
    1.3140    9166984   9. ErrorHandler->error() C:\xampp\htdocs\sandbox\cake\console\error.php:0


Notice: Undefined variable: code in C:\xampp\htdocs\sandbox\cake\console\error.php on line 68

Call Stack:
    0.0898     540608   1. {main}() C:\xampp\htdocs\sandbox\cake\console\cake.php:0
    0.1060     541392   2. ShellDispatcher->ShellDispatcher() C:\xampp\htdocs\sandbox\cake\console\cake.php:660
    0.4175    2728408   3. ShellDispatcher->dispatch() C:\xampp\htdocs\sandbox\cake\console\cake.php:139
    1.2805    9165232   4. usoShell->main() C:\xampp\htdocs\sandbox\cake\console\cake.php:377
    1.2805    9165232   5. usoCommonComponent->uho() C:\xampp\htdocs\sandbox\uso\vendors\shells\uso.php:30
    1.2805    9165736   6. Object->cakeError() C:\xampp\htdocs\sandbox\uso\controllers\components\uso_common.php:10
    1.2807    9165992   7. ErrorHandler->__construct() C:\xampp\htdocs\sandbox\cake\libs\object.php:201
    1.3140    9166968   8. call_user_func() C:\xampp\htdocs\sandbox\cake\console\error.php:56
    1.3140    9166984   9. ErrorHandler->error() C:\xampp\htdocs\sandbox\cake\console\error.php:0


Notice: Undefined variable: message in C:\xampp\htdocs\sandbox\cake\console\error.php on line 68

Call Stack:
    0.0898     540608   1. {main}() C:\xampp\htdocs\sandbox\cake\console\cake.php:0
    0.1060     541392   2. ShellDispatcher->ShellDispatcher() C:\xampp\htdocs\sandbox\cake\console\cake.php:660
    0.4175    2728408   3. ShellDispatcher->dispatch() C:\xampp\htdocs\sandbox\cake\console\cake.php:139
    1.2805    9165232   4. usoShell->main() C:\xampp\htdocs\sandbox\cake\console\cake.php:377
    1.2805    9165232   5. usoCommonComponent->uho() C:\xampp\htdocs\sandbox\uso\vendors\shells\uso.php:30
    1.2805    9165736   6. Object->cakeError() C:\xampp\htdocs\sandbox\uso\controllers\components\uso_common.php:10
    1.2807    9165992   7. ErrorHandler->__construct() C:\xampp\htdocs\sandbox\cake\libs\object.php:201
    1.3140    9166968   8. call_user_func() C:\xampp\htdocs\sandbox\cake\console\error.php:56
    1.3140    9166984   9. ErrorHandler->error() C:\xampp\htdocs\sandbox\cake\console\error.php:0

Error:
```


/cake/console/error.php の コンストラクタ内の下記の部分でしくじっているようです。

```
function __construct($method, $messages) {
    $this->stdout = fopen('php://stdout', 'w');
    $this->stderr = fopen('php://stderr', 'w');
    call_user_func_array(array(&$this, $method), $messages); //NG
}
```


引数 $method に "error" を、$messages に配列を入れても、なぜか error メソッドに渡る時点で、$messages の中身が最初の要素の文字列だけになってしまいます。

そこで、call\_user\_func\_array() ではなく call\_user_func() メソッドを使うと動くようになりました。

```
function __construct($method, $messages) {
    $this->stdout = fopen('php://stdout', 'w');
    $this->stderr = fopen('php://stderr', 'w');
    call_user_func(array($this, $method), $messages); // OK
}
```


この二つのメソッドの根本的な違いは何なのかわかりませんが、とりあえずこれでコンソール・コントローラから両方で正しく使えるようになりました。

検索してもあまり情報が出てきませんでした。詳しい原因を知っている人がいたら教えてください。

CakePHP のシェルに関するトラブルは概して情報量が少なくて困ります。

### 参考

  * [エラーハンドリング(Error Handling) :: CakePHPによる作業の定石 :: マニュアル :: 1.3コレクション :: The Cookbook](http://book.cakephp.org/ja/view/1188/Error-Handling)
  * [いつか辿りつこう(* ^ーﾟ) java seasar2 struts EL mysql many tips » CakePHP 例外処理](http://blog.hard-boiled.jp/?p=415)
  * [CakePHP＞エラーハンドリング(Error Handling) - 大人でも自由研究 - Everyone is creative. Originaly…!!](http://d.hatena.ne.jp/lifegood/20081008/p1)
  * [PHP: call_user_func_array - Manual](http://php.net/manual/ja/function.call-user-func-array.php)