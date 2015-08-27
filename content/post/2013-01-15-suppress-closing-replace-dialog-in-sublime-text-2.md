---
title: Sublime Text 2 の置換ダイアログを閉じないようにする
author: 1000k
layout: post
date: 2013-01-15
url: /2013/01/15/suppress-closing-replace-dialog-in-sublime-text-2/
categories:
  - その他
tags:
  - Sublime Text
  - カスタマイズ
---
最近は [Sublime Text 2](http://www.sublimetext.com/) をメインのエディタに使っています。しかし、ところどころ「何で？」と思うような不便な点があります。

「Replace All」をクリックするたびに閉じる置換ダイアログも主な不満の一つです。正規表現を連続で試したい時などはいちいち開き直さねばならず、面倒極まりないです。

というわけで、「Replace All」 を実行しても閉じないようにカスタマイズする方法をメモします。

<!--more-->

## やり方

_Preferences -> Key Bindings - User_ に以下の行を追加します。

```
{
        "keys": ["ctrl+alt+enter"],
        "command": "replace_all",
        "args": {"close_panel": false},
        "context": [
            {"key": "panel", "operand": "replace"},
            {"key": "panel_has_focus"}
        ]
    }
```


あとは置換ダイアログで「Ctrl+Alt+Enter」を叩けばOKです。置換してもダイアログは開きっぱなしになります。

ちなみに「"keys": ["enter"]」にすればデフォルトでダイアログを閉じないようにもできます。そこはお好みで。

## 参考

[Search and Replace panel should … / General forum / Sublime Text 2](http://sublimetext.userecho.com/topic/45051-search-and-replace-panel-should-not-hide-when-we-click-on-replace-all/)