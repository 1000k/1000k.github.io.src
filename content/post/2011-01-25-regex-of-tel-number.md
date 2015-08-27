---
title: 電話番号チェックする正規表現(入力枠1個、ハイフン抜き)
author: 1000k
layout: post
date: 2011-01-25
url: /2011/01/25/regex-of-tel-number/
categories:
  - PHP
tags:
  - PHP
  - 正規表現
  - 電話番号
---
「電話番号 正規表現」で検索するといろいろな正規表現パターンが出てきますが、完璧なものはあまり無いようです。私が必要としていた簡単なものもなかったので、PHPで書いてみました。

<!--more-->

どういう条件で使えるかコメントに書いたので、よくご確認ください。総務省の電話番号ルールに従っています。

```
/**
 * 電話番号が正しいか判定する
 *
 * - 入力枠1個、ハイフン抜きでチェックする
 * - 0A0, 0AB, 0ABC (A,B,Cは0以外)から始まる番号だけに対応
 *   - 「00XY-0市外局番-市内局番-加入者番号」の形式は非対応 (@seeのQ3参照)
 * - 10桁(固定電話)と11桁(固定電話)のみ対応
 *
 * @param  string $tel  電話番号
 * @return bool   判定結果 (true:正しい電話番号)
 * @see <a href="http://www.soumu.go.jp/main_sosiki/joho_tsusin/top/tel_number/q_and_a-2001aug.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.soumu.go.jp/main_sosiki/joho_tsusin/top/tel_number/q_and_a-2001aug.html', '電話番号に関するＱ＆Ａ']);" >電話番号に関するＱ＆Ａ</a>
 */
function is_valid_tel_number($tel = "") {
    return preg_match("/^0\d{9,10}$/", str_replace("-", "", $tel));
}
```


動作チェックは書きの通りです。

```
<?php
/**
 * 電話番号が正しいか判定する
 *
 * - 入力枠1個、ハイフン抜きでチェックする
 * - 0A0, 0AB, 0ABC (A,B,Cは0以外)から始まる番号だけに対応
 *   - 「00XY-0市外局番-市内局番-加入者番号」の形式は非対応 (@seeのQ3参照)
 * - 10桁(固定電話)と11桁(固定電話)のみ対応
 *
 * @param  string $tel  電話番号
 * @return bool   判定結果 (true:正しい電話番号)
 * @see <a href="http://www.soumu.go.jp/main_sosiki/joho_tsusin/top/tel_number/q_and_a-2001aug.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.soumu.go.jp/main_sosiki/joho_tsusin/top/tel_number/q_and_a-2001aug.html', '電話番号に関するＱ＆Ａ']);" >電話番号に関するＱ＆Ａ&lt;/a>
 */
function is_valid_tel_number($tel = "") {
    return preg_match("/^0\d{9,10}$/", str_replace("-", "", $tel));
}

$tels = array(
    // NGパターン
    "0",
    "1-1-1-1",
    "uhouho",
    "9999999999",       // 0から始まってない
    "03-333-333",       // ケタ不足
    "090-9999-999999",  // ケタ多すぎ
    "012301230123",     // ケタ多すぎ
    // OKパターン
    "03-3200-2222",     // 市外局番1桁: 東京など
    "043-000-0000",     // 市外局番2桁: 千葉
    "0166-00-0000",     // 市外局番3桁: 旭川
    "09913-0-0000",     // 市外局番4桁: 硫黄島
    "090-0000-0000",    // 携帯電話
    "050-0000-0000",    // 携帯電話
    "080-0000-0000",    // 携帯電話
);

foreach ($tels as $key => $tel) {
    echo "{$key}: {$tel} ... ";
    echo is_valid_tel_number($tel) ? "OK" : "NG";
    echo "\n";
}

 /** End of file */
```


出力結果

```
$ php tel_check.php
0: 0 ... NG
1: 1-1-1-1 ... NG
2: uhouho ... NG
3: 9999999999 ... NG
4: 03-333-333 ... NG
5: 090-9999-999999 ... NG
6: 012301230123 ... NG
7: 03-3200-2222 ... OK
8: 043-000-0000 ... OK
9: 0166-00-0000 ... OK
10: 09913-0-0000 ... OK
11: 090-0000-0000 ... OK
12: 050-0000-0000 ... OK
13: 080-0000-0000 ... OK
```


## 参考

  * <a href="http://www.soumu.go.jp/main_sosiki/joho_tsusin/top/tel_number/q_and_a-2001aug.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.soumu.go.jp/main_sosiki/joho_tsusin/top/tel_number/q_and_a-2001aug.html', '電話番号に関するＱ＆Ａ']);" title="電話番号に関するＱ＆Ａ">電話番号に関するＱ＆Ａ</a>