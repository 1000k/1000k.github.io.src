---
title: var_dump()の結果をファイルに出力する
author: 1000k
layout: post
date: 2011-11-04
url: /2011/11/04/export-var-dump-result-into-file/
categories:
  - PHP
tags:
  - PHP
  - TIPS
---
file\_put\_contents()とvar_export()を組み合わせれば、1行で実現できます。

# コード

```
/**
 * 変数のvar_dump()結果をログファイルに出力する。
 *
 * @param mixed $var ダンプしたい変数
 * @param string $path ログファイルのパス
 * @return int ファイルに書き込まれたバイト数。失敗した場合falseを返す。
 */
public function varExportToLog($var, $path) {
    return file_put_contents($path, $var_export($var, true), FILE_APPEND);
}
```


# 参考

  * [PHPメモ：var_export() - まんじうこわい@楽 - 楽天ブログ（Blog）](http://plaza.rakuten.co.jp/hknopage/diary/201109060000/)