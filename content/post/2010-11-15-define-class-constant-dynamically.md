---
title: クラス定数を動的に指定する
author: 1000k
layout: post
date: 2010-11-15
url: /2010/11/15/define-class-constant-dynamically/
categories:
  - PHP
tags:
  - PHP
  - クラス定数
  - トラブルシューティング
  - 可変変数
  - 定数
---
PHP で可変変数を使えば、動的に変数名や関数名を指定できます。簡単な例だと、下のようなことができます。

```
$a = 'hello';
${$a} = ' world!';  // $hello 変数に ' world!'; が格納される
echo $a . $hello;   // 'hello world!' と出力される
```


で、これを使って const で指定したクラス定数を動的に読み込みたかったのですが、なかなかうまく行きませんでした。

```
<?php
class Uso {
    const PRODUCTION_VALUE = 1;
    const DEVELOP_VALUE = 2;
}

$uso = new Uso();
$environment = $argv[1];

echo $uso::${$environment}_VALUE;   // Parse Error
```



<p>
  理由は、そもそもクラス**定数**であって**変数**ではないからでしょうか。
</p>


<p>
  とりあえず、**constant(定数名)**関数を使うことでうまくクラス定数を指定できました。
</p>


<p>
  <!--more-->
</p>


<p>
  使用例<br />
  --------<br />
  なんでこんな記事を書いたかというと、開発環境と本番環境の DB を簡単に接続しわけるクラスを作りたかったからです。<br />
  以下のクラスをrequireして、「MyConnections::get_connection("production")」と呼べば本番環境に接続したPDOオブジェクトを取得できます。
</p>


```

<?php
/**
 * DBに接続するクラス
 */

class MyConnections
{
    /** @var 本番環境の設定 */
    const PRODUCTION_DSN      = "mysql:host=productiondb; port=3306; dbname=uso_db";
    const PRODUCTION_USERNAME = "uso";
    const PRODUCTION_PASSWORD = "800";

    /** @var ローカル開発環境の設定 */
    const LOCAL_DSN      = "mysql:host=localhost; port=3306; dbname=uso_db;";
    const LOCAL_USERNAME = "root";
    const LOCAL_PASSWORD = "";

    /**
     * DBに接続し、PDOオブジェクトを返す
     *
     * @param  string  $target  接続先 (production|local)
     * @return PDO
     */
    public static function get_connections($target)
    {
        $env = strtoupper($target);

        $dsn      = constant("self::" . "{$env}_DSN");
        $username = constant("self::" . "{$env}_USERNAME");
        $password = constant("self::" . "{$env}_PASSWORD");

        try {
            return new pdo($dsn, $username, $password);
        } catch (PDOException $e) {
            throw $e;
        }
    }
}
?>
```



<h2>
  参考
</h2>


<ul>
  <li>
    <a href="http://jp.php.net/manual/ja/language.variables.variable.php" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://jp.php.net/manual/ja/language.variables.variable.php', 'PHP: 可変変数 - Manual']);" title="PHP: 可変変数 - Manual">PHP: 可変変数 - Manual</a>
  </li>


  <li>
    <a href="http://php.net/manual/ja/function.constant.php" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://php.net/manual/ja/function.constant.php', 'PHP: constant - Manual']);" title="PHP: constant - Manual">PHP: constant - Manual</a>
  </li>


  <li>
    <a href="http://www.shigeweb.jp/php/project_p/?page=static&section=php5oop" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.shigeweb.jp/php/project_p/?page=static&section=php5oop', '【 ほでなすPHP 】 PHP5の基本 -> スタティックメンバ／クラス定数']);" title="【 ほでなすPHP 】 PHP5の基本 -> スタティックメンバ／クラス定数">【 ほでなすPHP 】 PHP5の基本 -> スタティックメンバ／クラス定数</a>
  </li>


  <li>
    <a href="http://masha.maakikaku.jp/2008/03/php.php" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://masha.maakikaku.jp/2008/03/php.php', '[PHP][可変変数] 変数名や関数名を動的に指定する (masha.webTechLog)']);" title="[PHP][可変変数] 変数名や関数名を動的に指定する (masha.webTechLog)">[PHP][可変変数] 変数名や関数名を動的に指定する (masha.webTechLog)</a>
  </li>

</ul>