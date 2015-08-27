---
title: '[PHP] PHPの閉じタグは書いちゃダメ'
author: 1000k
layout: post
date: 2010-05-24
url: /2010/05/24/dont-write-php-closing-tag/
categories:
  - PHP
tags:
  - PHP
  - バグ
---
# 現象

```
<?php

// ... 処理 ...

?>
←改行1
←改行2
```


上のコードにブラウザからアクセスしてみると、改行が一つだけあるソースが表示される。PHPは閉じタグ直後の改行1個は自動で消してくれるが、2個目以上はそのまま表示するらしい。

こんなソースを header関数などを使うクラスからIncludeすると終わり。header関数の前にはいかなる出力もしてはいけないのに、勝手に改行が出力されているので、エラーが発生する。

閉じタグは書かなくてもいい。というか、書くと余計なエラーの原因になるので、**PHPの閉じタグ書くべきではない。**

# 参考

<a href="http://d.hatena.ne.jp/fbis/20090716/1247714151" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://d.hatena.ne.jp/fbis/20090716/1247714151', 'PHPの閉じタグは心の臓に悪いから使わないで &#8211; Unknown::Programming']);" title="PHPの閉じタグは心の臓に悪いから使わないで - Unknown::Programming">PHPの閉じタグは心の臓に悪いから使わないで &#8211; Unknown::Programming</a>