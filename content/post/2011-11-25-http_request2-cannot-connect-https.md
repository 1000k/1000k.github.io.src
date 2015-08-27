---
title: HTTP_Request2でhttps接続できない場合の対策
author: 1000k
layout: post
date: 2011-11-25
url: /2011/11/25/http_request2-cannot-connect-https/
categories:
  - PHP
tags:
  - https
  - PEAR
  - PHP
  - トラブルシューティング
---
HTTP_Request2::send()でhttpsサイトに接続しようとしたら、下記のようなエラーが返ってきました。

```
Malformed response:
Unable to connect to ssl://www.google.com:443.
stream_socket_client(): unable to connect to ssl://www.google.com:443 (Unknown error)
```


Unknown errorというのが一番タチが悪い。エラーが追えません。

検証のためcurlコマンドで同じURLに接続を試みると、ちゃんとレスポンスが返ってきます。何か設定をし忘れているに違いない。

# 原因

どうやら HTTP_Request::setConfig() で正しい接続設定を行えていない場合、"Malformed response" というエラーが出るようです。

私の場合、オレオレ証明書の許可と社内プロキシの設定を行っていなかったためにはまってしまったようです。

# 対応

オレオレ証明書を持ったサイトを許可するために、setConfig() で **ssl\_verify\_peer** と **ssl\_verify\_host** を false に設定してやります。

また、プロキシの設定も行います。

```
<?php
require_once('HTTP/Request2.php');
$req = new HTTP_Request2();

$req->setConfig(array(
    'ssl_verify_host' => false,
    'ssl_verify_peer' => false,
    'proxy_host' => 'uso.proxy.jp',
    'proxy_port' => '8080',
));

$response = $req->setUrl("https://www.google.com/")->send();
print_r($response);     // HTTPレスポンスが出力される
```


ほかにも設定項目はあるので、[マニュアル](http://pear.php.net/manual/en/package.http.http-request2.config.php)を参考に設定を疑ってみてはどうでしょうか。

# 参考

  * [Manual :: Configuration parameters for HTTP_Request2](http://pear.php.net/manual/en/package.http.http-request2.config.php)
      * setConfigで設定できる項目一覧
  * [HTTP_Request2 でのSSL通信でのPOST送信 - クマーのえんじにありんぐ](http://d.hatena.ne.jp/vivid_skid/20110822/1314013148)
      * オレオレ証明書の許可ではまってしまった方はこちら