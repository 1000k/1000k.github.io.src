---
title: '[その他] Poderosaマクロ – telnet自動ログイン'
author: 1000k
layout: post
date: 2010-05-16
url: /2010/05/16/telnet-auto-login-macro-in-poderosa/
categories:
  - JavaScript
tags:
  - etc
  - JavaScript
  - Poderosa
---
  * ワンクリックでtelnetログインし、自動でID/PWを入力するマクロ
  * いじるファイル：　Poderosaインストールディレクトリ > Macro > AutoLogin.js
  * 登録方法：　[ツール] > [マクロ] > [環境設定] > [新規]　で上記ファイルを指定

```
//AutoLogin.js
import Poderosa;
import Poderosa.ConnectionParam;
import Poderosa.Terminal;
import Poderosa.Macro;
import Poderosa.View;
import System.Drawing;
import System.Threading;

var vars = new Object();
// telnetの場合
connect("ホスト名", ConnectionMethod.Telnet, 23, EncodingType.UTF8, "ログインID", "ログインPW");
// SSHの場合
//connect("ssh-host", ConnectionMethod.SSH2, 22, EncodingType.EUC_JP, "ログインID", "ログインPW");

function connect(host, method, port, encoding, id, password) {
    vars.env = new Environment();
    if (method == ConnectionMethod.Telnet) {
        vars.param = new TelnetTerminalParam(host);
    } else {
        vars.param = new SSHTerminalParam(method, host, id, password);
    }
    vars.param.Port = port;
    vars.param.Encoding = encoding;
    vars.connection = vars.env.Connections.Open(vars.param);
    if (method == ConnectionMethod.Telnet) {
        wait("login: ");
        sendln(id);
        wait("Password: ");
        sendln(password);
    }
}

function sendln(s) {
    vars.connection.TransmitLn(s);
}

function wait(s) {
    Thread.Sleep(10);
    var r = vars.connection.ReceiveData();
    while(r.indexOf(s) == -1) {
        Thread.Sleep(10);
        r += vars.connection.ReceiveData();
    }
}
```
