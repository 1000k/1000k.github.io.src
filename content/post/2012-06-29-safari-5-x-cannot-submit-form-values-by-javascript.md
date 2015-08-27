---
title: Safari 5系で、JavaScriptでformの値をsubmitできない不具合の対策
author: 1000k
layout: post
date: 2012-06-29
url: /2012/06/29/safari-5-x-cannot-submit-form-values-by-javascript/
categories:
  - JavaScript
tags:
  - JavaScript
  - Safari
  - トラブルシューティング
---
Safari 5.x系では、JavaScriptで window.onload を使って自動でフォームの内容を送信（submit）しても、正しく値を渡せないバグがあります。

現象と対策を整理しました。

<!--more-->

具体的には下記のようなコードがうまくいきません。

page1.php -> page2.php -> page3.php の順に、usoFormの値をJSで自動でPOSTしています。

_page1.php_

```
```


_page2.php_

```
```


_page3.php_

```
```
<?php print_r($_POST); ?>```



<p>
  正常なブラウザなら page3.php で表示される uso の値は「801」ですが、Safari 5.xの場合「800」が表示されます。<br />
  page2でPOSTしている値が、page1のもので上書きされてしまっているようです。
</p>


<h2>
  原因
</h2>


<p>
  下記のスレッドの内容は私が遭遇したケースと全く同じでした。<br />
  どうやら原因はSafariの自動補完機能のようです。
</p>


<p>
  <a href="http://stackoverflow.com/questions/10546653/what-is-the-work-around-for-a-weird-javascript-form-submit-bug-in-safari-5" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://stackoverflow.com/questions/10546653/what-is-the-work-around-for-a-weird-javascript-form-submit-bug-in-safari-5', 'What is the work around for a weird javascript form submit bug in Safari 5? &#8211; Stack Overflow']);" title="What is the work around for a weird javascript form submit bug in Safari 5? - Stack Overflow">What is the work around for a weird javascript form submit bug in Safari 5? &#8211; Stack Overflow</a>
</p>


<blockquote>
  <p>
    Safari is filling in the form field using autofill behaviour. You probably have the &#8220;Other forms&#8221; option enabled in your preferences. Even Apple recommend you turn this feature off since it can be used for stealing private information.

  </p>
</blockquote>


<h2>
  対策
</h2>


<p>
  上記のベストアンサーでは、<strong>formタグにautocomplete=&#8221;off&#8221;を指定してやれば直る</strong>とアドバイスされています。<br />
  実際に試してみたところ、正しくPOSTできるようになりました。
</p>


```




```



<h2>
  余談（文句）
</h2>


<p>
  問題が確認できたバージョンは、5.0.1, 5.1.4, 5.1.5 でした。<br />
  一度直ったバグが再発しているところを見ると、Safariの開発体制ではバグを正しく管理できていないようです。<br />
  フォーム送信が失敗するなんて、ブラウザとして致命的のバグなのですが。
</p>


<h2>
  参考
</h2>


<ul>
  <li>
    <a href="http://stackoverflow.com/questions/10546653/what-is-the-work-around-for-a-weird-javascript-form-submit-bug-in-safari-5" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://stackoverflow.com/questions/10546653/what-is-the-work-around-for-a-weird-javascript-form-submit-bug-in-safari-5', 'What is the work around for a weird javascript form submit bug in Safari 5? &#8211; Stack Overflow']);" title="What is the work around for a weird javascript form submit bug in Safari 5? - Stack Overflow">What is the work around for a weird javascript form submit bug in Safari 5? &#8211; Stack Overflow</a>
  </li>


  <li>
    <a href="http://mac.softpedia.com/progChangelog/Safari-Changelog-25616.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://mac.softpedia.com/progChangelog/Safari-Changelog-25616.html', 'Safari 5.1.7 &#8211; Changelog &#8211; Softpedia']);" title="Safari 5.1.7 - Changelog - Softpedia">Safari 5.1.7 &#8211; Changelog &#8211; Softpedia</a>
    <ul>
      <li>
        Safariの更新履歴一覧。
      </li>

    </ul>

  </li>

</ul>