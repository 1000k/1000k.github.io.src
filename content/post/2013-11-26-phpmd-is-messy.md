---
title: PHPMD の頭がかなり messy
author: 1000k
layout: post
date: 2013-11-26
url: /2013/11/26/phpmd-is-messy/
categories:
  - PHP
tags:
  - PHP
  - PHPMD
  - トラブルシューティング
---
<a href="http://phpmd.org/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://phpmd.org/', 'PHPMD (PHP Mess Detector)']);" >PHPMD (PHP Mess Detector)</a> は、PHP のコードを静的解析して、潜在的なバグや無意味なブロックを発見してくれるツールです。

が、しばしば問題ないブロックにもケチをつけてきます。

たとえばこんなコード。

```
public function setUp() {
    // 警告: Avoid using static access to class 'parent' in method 'setUp'.
    parent::setUp();

    $this->Account = ClassRegistry::init('Account');
}
```


親クラスの setUp() メソッドを呼んだけで、「静的アクセスするな」と怒られます。じゃあどうやってオーバーライドすればいいんだよ、と思います。メンバ定数にアクセスしようと `self::` を使っても出ます。オブジェクト指向を否定してんのか。

`@SuppressWarnings` アノテーションをクラスの PHPDoc に書くことでこの警告は出なくなりますが、本当に間違った静的アクセスを行っていても無視されるようになってしまいます。困ったもんです。

ほかにも、if-else を使うと下のような警告を出します。

```
public function step() {
    if ($this->find('count') &lt; 1) {
        $this->foo();
    } else {
        // 警告: The method step uses an else expression. Else is never necessary and you can simplify the code to work without else.
        $this->bar();
    }
```


「else なんて必要ねえよ。else の必要ない書き方しろよ。」と言ってきます。言いたいことはわかりますが、リファクタリングした結果最もメンテナンス性が高くなるなら、if-else 構文を使っても全然問題ないはずです。else を使うだけで警告レベル High で糾弾するのはいかがなものでしょうか。

## まとめ

PHPMD はヒステリー気味なので、レポート結果をあまり深刻に受け取らないようにしましょう。

## 参考

  * <a href="http://phpmd.org/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://phpmd.org/', 'PHPMD &#8211; PHP Mess Detector']);" >PHPMD &#8211; PHP Mess Detector</a>
  * <a href="http://stackoverflow.com/questions/18604179/phpmd-avoid-static-access-to-parent" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://stackoverflow.com/questions/18604179/phpmd-avoid-static-access-to-parent', 'php &#8211; PHPMD avoid static access to parent &#8211; Stack Overflow']);" >php &#8211; PHPMD avoid static access to parent &#8211; Stack Overflow</a>