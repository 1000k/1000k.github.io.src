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
最近は <a href="http://www.sublimetext.com/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.sublimetext.com/', 'Sublime Text 2']);" title="Sublime Text: The text editor you'll fall in love with">Sublime Text 2</a> をメインのエディタに使っています。しかし、ところどころ「何で？」と思うような不便な点があります。

「Replace All」をクリックするたびに閉じる置換ダイアログも主な不満の一つです。正規表現を連続で試したい時などはいちいち開き直さねばならず、面倒極まりないです。

というわけで、「Replace All」 を実行しても閉じないようにカスタマイズする方法をメモします。

<!--more-->

## やり方

_Preferences -> Key Bindings &#8211; User_ に以下の行を追加します。

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

ちなみに「&#8221;keys&#8221;: [&#8220;enter&#8221;]」にすればデフォルトでダイアログを閉じないようにもできます。そこはお好みで。

## 参考

<a href="http://sublimetext.userecho.com/topic/45051-search-and-replace-panel-should-not-hide-when-we-click-on-replace-all/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://sublimetext.userecho.com/topic/45051-search-and-replace-panel-should-not-hide-when-we-click-on-replace-all/', 'Search and Replace panel should &#8230; / General forum / Sublime Text 2']);" title="Search and Replace panel should ... / General forum / Sublime Text 2">Search and Replace panel should &#8230; / General forum / Sublime Text 2</a>