---
title: CakePHP のコントローラを単純化するために例外を使う
author: 1000k
layout: post
date: 2010-07-23
url: /2010/07/23/simplify-controller-by-exception/
categories:
  - CakePHP
  - PHP
  - テスト
tags:
  - CakePHP
  - Exceptions
  - UnitTest
  - 翻訳
---
2009年6月12日の記事でだいぶ古いのですが、CakePHP のコードを改善する TIPS があったので訳してみました。

<a href="http://mark-story.com/posts/view/simplifying-controller-logic-with-exceptions" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://mark-story.com/posts/view/simplifying-controller-logic-with-exceptions', 'Simplifying Controller logic with Exceptions | Mark Story']);" title="Simplifying Controller logic with Exceptions | Mark Story">Simplifying Controller logic with Exceptions | Mark Story</a>

要約すると、**「モデル内で例外を吐くようにすると、エラーコードが読みやすくなり、コントローラのテストもしやすくなるよ」**ということです。

以下、訳です。

<!--more-->

増大するコードと格闘する日々・・・どうにかクリエイティブな方法で解決したい。そんな悩みを、モデルのメソッドから例外を投げる方法で解決しました。別にビックリするようなものではないです。ただfalseを返すより、ちょっと便利な点がいくつかあります。

第一に、if-elseを減らすことができます。第二に、エラーが起こる部分のソースにエラーメッセージを書くことができるので、何度も使うメソッドならエラーメッセージを重複させずに書くことができます。

例えば以下のメソッドは、リモートアドレスからリソースをダウンロードし、

ローカルファイルシステムとデータベースに記録するものです。_（訳注: ダウンロードの時点で失敗すると例外を、保存に成功するとtrueを、保存に失敗するとfalseを返します。）_

```
public function downloadResource($url, $userId, $type = 'image') {
    if (!isset($this->_fetchMimes[$type])) {
        throw new OutOfBoundsException(__('Invalid media type', true));
    }
    $this->_loadSocket($url);
    $resource = $this->Socket->get($url);
    if (!isset($this->Socket->response['header']['Content-Type'])) {
        throw new OutOfBoundsException(__('Submitted url has no Mime-Type', true));
    }
    $allowedContentTypes = $this->_fetchMimes[$type];
    if (!in_array($this->Socket->response['header']['Content-Type'], $allowedContentTypes)) {
        throw new OutOfBoundsException(__('Submitted url has an invalid Mime-Type', true));
    }
    $newFile = array(
        'File' => array(
            'file' => $this->_saveFetchedFile($resource, $url, $this->Socket->response['header']['Content-Type']),
            'user_id' => $userId,
            'title' => $url,
        )
    );
    $this->create($newMedia);
    if ($this->save()) {
        return true;
    }
    return false;
}
```


見てわかる通り、起きてはならないことが起きた時には OutOfBoundsExceptions を投げています。最近のバージョンの PHP に含まれている SPL ライブラリには、便利なクラスが多数用意されています。もちろん自分で例外を作ることもできますが、たいていは組み込みの例外を使うだけで十分でしょう。

これを使えば、コントローラのメソッドをかなりスッキリさせることができます。何重もの if でチェックする必要が無くなります。また、シンプルでエラーの見通しも良いコードを書くことができます。

```
try {
    $this->File->downloadResource($this->data['File']['url'], $this->Auth->user('id'), 'image');
    //do some additional file handling and data processing.

    $this->Session->setFlash(__('File uploaded successfully', true));
} catch(OutOfBoundsException $e) {
    $this->Session->setFlash($e->getMessage());
}
$this->redirect(array('action' => 'index'));
```


このメソッドの例外発生をテストするのは簡単です。assertFalse を使うのではなく、pass() と fail() を使えばいいのです。

```
// (訳注) 不正なURLを送った時に例外が発生することをテストする
try {
    $this->File->downloadResource('http:/bogus.com/', 1, 'image');  // 間違ったURL
    $this->fail('No exception thrown with bogus arguments');
} catch (Exception $e) {
    $this->pass('Exception thrown');
}
```


例外を使うと便利になることは多いです。だからと言って、false を返すタイプのメソッドすべてを例外を返すようにする必要はないでしょう。たとえばヘルパから例外を返すようにすると、満足より苦痛を感じることのほうが多くなるでしょう。

どんなツールもそうですが、正しく使えばメンテナンス性の良いコードを生み出すことができるのです。