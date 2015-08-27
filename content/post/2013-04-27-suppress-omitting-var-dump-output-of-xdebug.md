---
title: Xdebug 使用時に var_dump の出力が省略されないようにする
author: 1000k
layout: post
date: 2013-04-27
url: /2013/04/27/suppress-omitting-var-dump-output-of-xdebug/
categories:
  - PHP
  - PHPUnit
tags:
  - PHP
  - xDebug
  - トラブルシューティング
---
Xdebug をインストールしていると、var_dump() した変数が全て表示されずに省略されてしまうことがあります。

```
array(1187) {
  [0] =>
  string(19) "ALICE IN WONDERLAND"
  :
  :
  [127] =>
  string(4) "OLEO"

  (more elements)...
}
```


全て表示するには、`/etc/php.ini` に以下の行を追加すれば OK です。

```
; 表示する子要素の最大数 (default: 128)
xdebug.var_display_max_children = -1

; 表示する要素の最大数 (default: 512)
xdebug.var_display_max_data = -1

; 表示する最大の階層 (default: 3)
xdebug.var_display_max_depth = -1
```


## 参考

  * <a href="http://xdebug.org/docs/all_settings#var_display_max_depth" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://xdebug.org/docs/all_settings#var_display_max_depth', 'Xdebug: Documentation']);" >Xdebug: Documentation</a>
  * <a href="http://www.kantenna.com/info/2012/02/xdebug_vardump.php" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.kantenna.com/info/2012/02/xdebug_vardump.php', '[PHP]Xdebugでvar_dump()の出力が省略されて困る場合の対応 | 情報備忘録']);" >[PHP]Xdebugでvar_dump()の出力が省略されて困る場合の対応 | 情報備忘録</a>
  * <a href="http://www.crossl.net/blog/php_xdebug_vardump_size/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.crossl.net/blog/php_xdebug_vardump_size/', 'PHPでvar_dump時に数が多いと省略されてしまう現象']);" >PHPでvar_dump時に数が多いと省略されてしまう現象</a>