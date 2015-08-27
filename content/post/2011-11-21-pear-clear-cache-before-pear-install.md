---
title: pear installできない問題はpear clear-cacheで解決する
author: 1000k
layout: post
date: 2011-11-21
url: /2011/11/21/pear-clear-cache-before-pear-install/
categories:
  - PHP
tags:
  - PEAR
  - PHP
  - トラブルシューティング
---
pear installでパッケージをインストールしようと思いましたが、身に覚えのないエラーが出て困りました。

インストールエラーの原因と、解決法をメモしておきます。

<!--more-->

出てきたエラーメッセージは以下。

```
$ pear install HTTP_Request2
PHP_PEAR_PHP_BIN is not set correctly.
Please fix it using your environment variable or modify
the default value in pear.bat
The current value is:
.\php.exe

$ pear install Net_URL2-2.0.0
SECURITY ERROR: Will not write to C:\DOCUME~1\keijiro\LOCALS~1\Temp\pear\cache\ee3f372dc6f69cdae3c6b0c2a0d1098erest.cacheid as it is symlinked to C:\DOCUME~1\keijiro\LOCALS~1\Temp\pear\cache\ee3f372dc6f69cdae3c6b0c2a0d1098erest.cacheid - Possible symlink attack
install failed
```


「PHP\_PEAR\_PHP_BINが間違ってる」「一時ファイルが書き込めない」など言われていますが、どちらも正常に設定できています。

はてどうしたことか。

と思いましたが、**pear clear-cache**を実行したらコマンドが通るようになりました。

```
$ pear clear-cache
reading directory C:\DOCUME~1\keijiro\LOCALS~1\Temp\pear\cache
1024 cache entries cleared

$ pear install Net_URL2-2.0.0

Notice: fwrite(): send of 189 bytes failed with errno=10054 既存の接続はリモート ホストに強制的に切断されました。
 in PEAR\Downloader.php on line 1664
downloading Net_URL2-2.0.0.tgz ...
Starting to download Net_URL2-2.0.0.tgz (11,325 bytes)
.....done: 11,325 bytes
install ok: channel://pear.php.net/Net_URL2-2.0.0

$ pear install HTTP_Request2-2.0.0

Notice: fwrite(): send of 189 bytes failed with errno=10054 既存の接続はリモート ホストに強制的に切断されました。
 in PEAR\Downloader.php on line 1664
pear/HTTP_Request2 can optionally use PHP extension "curl"
pear/HTTP_Request2 can optionally use PHP extension "fileinfo"
downloading HTTP_Request2-2.0.0.tgz ...
Starting to download HTTP_Request2-2.0.0.tgz (97,476 bytes)
......................done: 97,476 bytes
install ok: channel://pear.php.net/HTTP_Request2-2.0.0
```


Noticeが出ていますが、パッケージは問題無く利用できました。

でたらめなエラーメッセージはやめていただきたいですね。

## 参考

  * [Security Error while installing phing using pear - Stack Overflow](http://stackoverflow.com/questions/6442187/security-error-while-installing-phing-using-pear)