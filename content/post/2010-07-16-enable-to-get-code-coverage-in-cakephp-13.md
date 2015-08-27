---
title: CakePHP1.3 + XAMPP 1.7.3でコードカバレッジを取得できるようにする
author: 1000k
layout: post
date: 2010-07-16
url: /2010/07/16/enable-to-get-code-coverage-in-cakephp-13/
categories:
  - CakePHP
  - PHP
tags:
  - CakePHP
  - xDebug
  - トラブルシューティング
---
CakePHPでは「http://{アプリURL}/test.php」で単体テスト用ページを表示できます。ここに「Analyze Code Coverage」なんてリンクがあるのですが、私の環境ではここをクリックするとApacheが強制終了させられて、コードカバレッジを取得できませんでした。

どうやらXAMPP 1.7.3に含まれているxDebugのバージョンが原因のようでした(v2.0.6 rc)。そこで最新のxDebug 2.1.0 を入れたところ、無事取得できるようになりました。

xDebugの入れ替え方ですが、[Xdebug: Support; Tailored Installation Instructions](http://www.xdebug.org/find-binary.php)にしたがってやるのが一番わかりやすいです。ここのフォームにphpinfoの結果をまるごとコピペするだけで、DLするファイルからphp.iniの設定まですべてやり方を出してくれます。

### 参考

  * [[CakePHP]晴れときどきcakephpでコードカバレージを測定する | Ryuzee.com](http://www.ryuzee.com/contents/blog/2430)
  * [Xdebug: Support; Tailored Installation Instructions](http://www.xdebug.org/find-binary.php)