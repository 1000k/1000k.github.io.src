---
title: 依存性注入の解説とやり方
author: 1000k
layout: post
date: 2012-10-24
url: /2012/10/24/lecture-of-dependency-injection/
categories:
  - PHP
  - PHPUnit
  - テスト
tags:
  - PHP
  - コラム
  - チュートリアル
  - テスト
  - 単体テスト
---
**依存性注入 (Dependency Injection)** は、クラスを単体テスト可能にするために使われるテクニックです。

これが意識されていないが故に単体テストが全くできないコードをよく見かけます。

単体テストの際には必ず必要になる知識なので、解説しておきます。

以下のサンプルでは <a href="http://www.phpunit.de/manual/3.8/ja/index.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.phpunit.de/manual/3.8/ja/index.html', 'PHPUnit']);" >PHPUnit</a> を利用しています。

<!--more-->

## 単体テストができないケース

例えば以下のメソッド Foo::play() は単体テストケースが書けません。

```
class Foo {
    public function play() {
        $bar = new Bar();

        if ($bar->getSomething() === 1) {
            return true;
        }

        return false;
    }
}
```


play() 内で外部クラス Bar をインスタンス化しています。つまり Foo::play() メソッドは Bar クラスに**依存**しており、単体テストができません。

このまま無理矢理テストケースを書くと、以下のようになります。

```
class FooTest {
    /**
     * @covers Foo::play
     */
    public function testPlay() {
        $foo = new Foo;
        $this->assertTrue($this->foo->play());
    }
}
```


何が問題かわかるでしょうか？Foo クラスのテスト結果は Bar クラスに依存してしまっています。つまり、Foo は何も変更していなくても、Bar を変更したタイミングで Foo の単体テストが壊れてしまう可能性があります。これでは結合テストであり、単体テストになっていません。

そのため、通常は Bar クラスを**モック化**し、Bar クラスの実装を無視できるようにします。

## モック化とは？

モック化とは、クラスの挙動を置き換えることです。例えば特定のメソッドの戻り値を好きな値に指定することができます。

今回のケースでは、Bar::getSomething() が常に 1 を返すようにすれば、「return true」の行が実行されることをテストできますし、1 以外を返すようにすれば「return false」の行が実行されることをテストできます。

しかし先述のコードのままでは、テスト対象のメソッド内で Bar をインスタンス化しているため、モックを注入する手段がありません。

まだピンと来ない方も、この先の具体例を見ればイメージが掴めると思います。

## メソッドを単体テスト可能にする

もしメソッドが下記のようになっていればどうでしょうか？

```
public function play(Bar $bar) {     // Bar クラスのインスタンスを受け取るようにした。
        if ($bar->getSomething() === 1) {
            return true;
        }

        return false;
    }
```


メソッド内で Bar クラスを new するのでなく、Bar インスタンスを引数に受け取って利用するだけにしました。これならテストケースでモックを注入することができます。

```
public function testPlay() {
        $foo = new Foo;

        $bar = $this->getMock('Bar');         // Bar クラスのモックを作成する。
        $bar->expects($this->any())
            ->method('getSomething')
            ->will($this->returnValue(1));    // Bar::getSomething() の戻り値が 1 になるよう設定する。

        $this->assertTrue($foo->play($bar));  // Foo::play() に注入
    }
```


Foo::play() メソッドは Bar クラスとの依存が解消され、単体テストが可能になりました。これでもう Bar クラスの実装を変更しても Foo クラスのテストが壊れることはありません。これが**依存性注入**です。

これはメソッドの引数にオブジェクトを渡せるようにする手法で、**インターフェース・インジェクション** (Interface Injection) と呼ばれます。

（※厳密なインターフェース・インジェクションでは、まずクラスの interface を作成し、それを委譲したメソッドを実装する必要があります。上記の例では簡略化のため interface は作成していません。）

## コンストラクター・インジェクション

依存性注入は以下のようなやり方でも実現が可能です。

```
class Foo {
    /** @var Bar */
    protected $bar;

    public function __constructor(Bar $bar = null) {
        // メンバ変数として Bar インスタンスを作成しておく。
        $this->bar = $bar ? $bar : new Bar;
    }

    public function play() {
        // メンバ変数の Bar インスタンスからメソッドを呼び出す。
        if ($this->bar->getSomething() === 1) {
            return true;
        }

        return false;
    }
}
```


このケースでは、Foo クラスのコンストラクタに Bar クラスのオブジェクトを渡せるようにしています。これは**コンストラクター・インジェクション** (Constructor Injection) と呼ばれるテクニックです。

この場合のテストケースは以下のようになります。

```
public function testPlay() {
        // Bar クラスのモックを作成する。
        $bar = $this->getMock('Bar');
        $bar->expects($this->any())
            ->method('getSomething')
            ->will($this->returnValue(1));

        $foo = new Foo($bar);   // Constructor Injection でモックを注入する。

        $this->assertTrue($foo->play($bar));
    }
```


※コンストラクタ内の3項演算子について補足。

```
public function __constructor(Bar $bar = null) {
        $this->bar = $bar ? $bar : new Bar;
    }
```


コンストラクタの引数に $bar が渡されなかった時はデフォルトの Bar クラスを new するようにしています。これにより、テストの時はモックを注入して使うようにし、本番コードではパラメータを何も指定しないことでデフォルトの Foo クラスを使うようにしています。

## セッター・インジェクション

別のアプローチとして、クラスにオブジェクトを注入するためセッターメソッドを用意する方法もあります。

```
class Foo {
    /** @var Bar */
    protected $bar;

    /**
     * セッターメソッド
     *
     * @param Bar $bar
     */
    public function setBar(Bar $bar) {
        $this->bar = $bar;
    }

    public function play() {
        if ($this->bar->getSomething() === 1) {
            return true;
        }

        return false;
    }
}
```


コンストラクターで Bar インスタンスを注入するのでなく、setBar() メソッドで Bar オブジェクトを注入しています。

この場合のテストケースは以下のようになるでしょう。

```
public function testPlay() {
        // Bar クラスのモックを作成する。
        $bar = $this->getMock('Bar');
        $bar->expects($this->any())
            ->method('getSomething')
            ->will($this->returnValue(1));

        $foo = new Foo;
        $foo->setBar($bar);     // セッター経由でモックを注入する。

        $this->assertTrue($foo->play($bar));
    }
```


これが**セッター・インジェクション** (Setter Injection) です。

## まとめ

  * 単体テストしたければ、メソッド内で外部クラスを new してはいけない。
  * モックを注入可能にするためには、以下のいずれか手法を採る。
      * メソッドの引数に使いたいオブジェクトを渡せるようにする。 (Interface Injection)
      * 使う予定のオブジェクトをコンストラクタに渡せるようにする。(Constructor Injection)
      * オブジェクトを setter メソッドで渡せるようにする。(Setter Injection)

## 補足1: runkitで強引にメソッドの挙動を変える方法

PHPにおいては <a href="http://php.net/manual/ja/book.runkit.php" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://php.net/manual/ja/book.runkit.php', 'runkit']);" >runkit</a> を使うことで、モック化をしなくてもメソッドの挙動を強引に変えることが可能です。ただし破壊的な手段であり、これが必要な時点でオブジェクト指向的な設計ができていない疑いが非常に強いです。

できるだけ Constructor Injection 等の方法を用い、テスト可能になるようリファクタリングをしましょう。リファクタリング中はコードが醜くなるかもしれませんが、最終的に設計が改善できるならば結果OKです。美しさよりも安全性が何より重要です。

## 補足2: Constructor Injection vs Setter Injection

<a href="http://blog.1000k.net/2012/10/23/%e6%8a%84%e8%a8%b3-constructor-injection-vs-setter-injection/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/2012/10/23/%e6%8a%84%e8%a8%b3-constructor-injection-vs-setter-injection/', '抄訳: Constructor Injection vs. Setter Injection | 1000g']);" >抄訳: Constructor Injection vs. Setter Injection | 1000g</a> にて、コンストラクター・インジェクションとセッター・インジェクションのどちらが良いかを考察しています。結論としてはコンストラクター・インジェクションがベターです。

## 参考

  * <a href="http://ja.wikipedia.org/wiki/%E4%BE%9D%E5%AD%98%E6%80%A7%E3%81%AE%E6%B3%A8%E5%85%A5" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://ja.wikipedia.org/wiki/%E4%BE%9D%E5%AD%98%E6%80%A7%E3%81%AE%E6%B3%A8%E5%85%A5', '依存性の注入 &#8211; Wikipedia']);" >依存性の注入 &#8211; Wikipedia</a>
  * <a href="http://d.hatena.ne.jp/m-hiyama/20060926/1159253903" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://d.hatena.ne.jp/m-hiyama/20060926/1159253903', 'DI（依存性注入）を白紙から説明してみる &#8211; 檜山正幸のキマイラ飼育記']);" >DI（依存性注入）を白紙から説明してみる &#8211; 檜山正幸のキマイラ飼育記</a>
  * <a href="http://martinfowler.com/articles/injection.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://martinfowler.com/articles/injection.html', 'Inversion of Control Containers and the Dependency Injection pattern']);" >Inversion of Control Containers and the Dependency Injection pattern</a>
      * リファクタリング界の大御所、Martin Fowler氏の考察。
  * <a href="http://www.techscore.com/tech/Java/Others/Spring/1-3/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.techscore.com/tech/Java/Others/Spring/1-3/', 'Dependency Injection のタイプ | TECHSCORE(テックスコア)']);" >Dependency Injection のタイプ | TECHSCORE(テックスコア)</a>
  * <a href="http://www.phpunit.de/manual/3.7/ja/test-doubles.html#test-doubles.mock-objects" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.phpunit.de/manual/3.7/ja/test-doubles.html#test-doubles.mock-objects', '第10章 テストダブル']);" >第10章 テストダブル</a>
      * PHPUnit 公式マニュアルによるモックの使い方。