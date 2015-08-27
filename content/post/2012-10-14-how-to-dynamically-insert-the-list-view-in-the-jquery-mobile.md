---
title: jQuery Mobileでリストビューを動的に挿入する方法
author: 1000k
layout: post
date: 2012-10-14
url: /2012/10/14/how-to-dynamically-insert-the-list-view-in-the-jquery-mobile/
categories:
  - JavaScript
  - jQuery
tags:
  - jQuery
  - jQuery Mobile
  - チュートリアル
---
jQuery Mobile 1.2 を使ってリストビューを動的に挿入する手順を紹介します。

[公式マニュアル](http://jquerymobile.com/test/docs/pages/page-dynamic.html)にもやり方は書いてあるのですが、本題と関係ない部分（リンクをクリックしたらその取得先を正規表現で取得するなど）が長く読みづらいので、もっと簡単なバージョンを書きました。

## やりたいこと

  1. 「Inject!」ボタンを押す
  2. 「data-role="content"」内にリストビューを挿入する
  3. 画面遷移せずに表示する

画面イメージはこんな感じです。↓

{{< img src="/img/dynamic_injection1.png" title="" >}}

「Inject!」をクリックすると、ヘッダとコンテンツが書き換えられます。

{{< img src="/img/dynamic_injection2.png" title="" >}}

<!--more-->

## サンプルコード

```







<div data-role="page" id="page1">
  <div data-role="header">
    <h1>
      Dynamic Injection
    </h1>

  </div>



  <div data-role="content">
    <a href="#" data-role="button" id="inject">Inject!</a>

  </div>

</div>


```


処理の内容を細かく見てみましょう。

### クリックイベントをバインドする

はじめに、「Inject!」ボタンをクリックした時に showList() 関数を呼び出すようにバインドします。

```
$("#inject").live("click", function() {
  showList();
});
```


次に showList() の中身を見てみます。

### DOM要素を格納する

まずはページ内の各要素のDOM要素を変数に格納しておきます。

```
var $page = $("#page1"),
      $header = $page.children(":jqmData(role=header)"),
      $content = $page.children(":jqmData(role=content)");
```


ちなみにこの「:jqmData()」はjQuery Mobile 用のカスタムセレクタです。

意味は「$page.childern("[data-role=header]")」と同じなのですが、jQM独自の「data-*」という要素をセレクトしたい場合はこちらを使う方が良いそうです。

詳しくは下記のマニュアルを参照してください。

[jqmData | jQuery Mobile 1.1.0 日本語リファレンス](http://dev.screw-axis.com/doc/jquery_mobile/api/methods_utilities/methods/jqmData/)

### リストビューを生成する

続いてリストビューのDOM要素を生成します。

jQueryのメソッドチェーンを使って作成すると楽です。

```
var $listview = $("

<ul>
  ").attr({"data-role":"listview", "data-inset":"true"})
      .append(
        $("

  <li>
    ").attr({"data-wrapperrels":"div", "data-iconpos":"right", "data-icon":"arrow-l"})
            .append($("<a>").attr({"href":"#", "title":"Home", "data-rel":"back"})
              .append($("

    <h3>
      ").text("FOO"))
                .append($("").attr({"class":"ui-li-aside ui-li-desc"}).text("BAR"))
              )
          ).appendTo(":jqmData(role=content)");
      ```



      <p>
        見慣れないと読みにくいかもしれませんが、インデントで階層構造が表現できて楽です。<br />
        これで実際に作成されるHTMLは以下のようになります。
      </p>


      ```



<ul data-role="listview" data-inset="true">
  <li data-icon="arrow-r" data-iconpos="right" data-wrapperrels="div">
    <a href="#">


    <h3>
      FOO
    </h3>


    <p class="ui-li-aside ui-li-desc">
      BAR
              </a>
          </li>
      </ul>
      ```



      <p>
        なお、この形式のリストは公式ドキュメントでは「Formatted content」と呼ばれています。説明ページは下記。
      </p>


      <p>
        [jQuery Mobile Docs - Lists with Form Controls](http://jquerymobile.com/test/docs/lists/lists-inset.html)
      </p>


      <h3>
        DOM要素を変更＆挿入する
      </h3>


      <p>
        ページの要素を書き換えます。
      </p>


      ```

  $header.find("h1").html("Injected");
  $content.html($listview);

  $page.page();
  $content.find(":jqmData(role=listview)").listview();
```



      <p>
        「$page.page()」は、遅延してページ要素を編集した後には必ず呼び出さねばならないようです。<br />
        その後、「$content.find(":jqmData(role=listview)").listview();」でリストビューをスタイリングしています。
      </p>


      <h3>
        ページを表示する
      </h3>


      <p>
        最後にページ遷移を手動で発生させます。<br />
        これまでに編集したDOM要素が表示されます。
      </p>


      ```

  $.mobile.changePage($page);
```



      <h2>
        補足
      </h2>


      <p>
        [公式ドキュメントのチュートリアル](http://jquerymobile.com/test/docs/pages/page-dynamic.html)ではここで紹介した内容にプラスして、「クリックしたボタンのhref属性を解析してデータ取得先を分ける」手順が説明されています。むしろその部分が長すぎて理解しづらくなっている気がします。
      </p>


      <h2>
        参考
      </h2>


      <ul>
        <li>
          [jQuery Mobile Docs - Dynamically Injecting Pages](http://jquerymobile.com/test/docs/pages/page-dynamic.html)
          <ul>
            <li>
              公式のチュートリアル。長い。
            </li>

          </ul>

        </li>


        <li>
          [jQuery Mobile Docs - Methods](http://jquerymobile.com/test/docs/api/methods.html)
          <ul>
            <li>
              $.mobile.changePage() のリファレンス。
            </li>

          </ul>

        </li>


        <li>
          [jqmData | jQuery Mobile 1.1.0 日本語リファレンス](http://dev.screw-axis.com/doc/jquery_mobile/api/methods_utilities/methods/jqmData/)
          <ul>
            <li>
              カスタムセレクタ「jqmData」の説明。
            </li>

          </ul>

        </li>


        <li>
          [リストビューについて | jQuery Mobile 1.1.0 日本語リファレンス](http://dev.screw-axis.com/doc/jquery_mobile/components/lists/docs/)
        </li>


        <li>
          [jQuery Mobile: Organizing Information with List Views | Packt Publishing](http://www.packtpub.com/article/jquery-mobile-organizing-information-with-list-views)
          <ul>
            <li>
              公式ドキュメントのリストビューの解説ではコードが書いてありませんが、このページには書いています。
            </li>

          </ul>

        </li>


        <li>
          [Building HTML (~= DOM) using jQuery - Stack Overflow](http://stackoverflow.com/questions/805159/building-html-dom-using-jquery)
          <ul>
            <li>
              jQueryのメソッドチェーンを使ってDOMを組み立てる方法。
            </li>

          </ul>

        </li>

      </ul>