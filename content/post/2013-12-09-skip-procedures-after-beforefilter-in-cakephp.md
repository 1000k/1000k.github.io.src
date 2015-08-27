---
title: CakePHP で beforeFilter() 以降の処理を実行しないようにする方法
author: 1000k
layout: post
date: 2013-12-09
url: /2013/12/09/skip-procedures-after-beforefilter-in-cakephp/
categories:
  - CakePHP
  - PHP
tags:
  - CakePHP
  - PHP
  - TIPS
---
サービスをメンテナンス中には、どこのページにアクセスされてもメンテナンス用画面を表示できるようにしたいでしょう。

例えば CakePHP で API を作ったなら、メンテナンス時にはどのページにアクセスしても以下のようなレスポンスが返ってくるようにしたい。

```
{
    "result": "NG",
    "message": "This service is now under maintenance."
}
```


これを実現するためのコードを考えました。

なお、CakePHP 2.4.2 で動作確認済みです。

<!--more-->

## やりたいこと

メンテナンスモードが ON の時は、どのアクションを叩かれても JSON でエラーレスポンスを返すようにする。

## コード

```
App::uses('Controller', 'Controller');

class AppController extends Controller {
/**
 * beforeFilter
 * サービスがメンテナンス中なら、メンテナンス画面を描画する。
 */
    public function beforeFilter() {
        if ($this->isUnderMaintenance()) {
            $this->set('output', [
                'result' => 'NG',
                'message' => 'This service is now under maintenance.'
            ]);
        }
    }

/**
 * beforeRender
 * ini/json形式の出し分けをする。
 */
    public function beforeRender() {
        $this->viewClass = 'Json';
        $this->set('_serialize', 'output');
    }

/**
 * メンテナンスモード時はアクションを実行しないようにする。
 *
 * @param CakeRequest $request
 */
    public function invokeAction(CakeRequest $request) {
        if ($this->isUnderMaintenance()) {
            return false;
        }

        return parent::invokeAction($request);
    }

/**
 * メンテナンスファイルが存在しているとメンテナンス中。
 *
 * @return boolean メンテナンス中なら true
 */
    public function isUnderMaintenance() {
        return file_exists('/tmp/maintenanceFile');
    }
}
```


## ポイント

  * `AppController::beforeFilter()` に実装することで、全てのコントローラーに影響させることができます。
  * `invokeAction()` をオーバーライドして、メンテナンスモード時にはアクションを実行させないようにしています。これが無いと、画面にはメンテナンス用の表示がされていても、裏側では結局アクションが実行されてしまいます。

## 余談

本当は CakePHP 2.1 から採用された <a href="http://book.cakephp.org/2.0/en/core-libraries/events.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://book.cakephp.org/2.0/en/core-libraries/events.html', 'Events System']);" >Events System</a> を使って `beforeFilter()` 以降のアクションを実行されないようにしたかったのですが、やり方がわかりませんでした。アクションを detach する方法をご存知の方がいたら教えて下さい。

## 参考

  * <a href="http://book.cakephp.org/2.0/en/views/json-and-xml-views.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://book.cakephp.org/2.0/en/views/json-and-xml-views.html', 'JSON and XML views — CakePHP Cookbook v2.x documentation']);" >JSON and XML views — CakePHP Cookbook v2.x documentation</a>
      * ビューを使わず JSON で出力を行う方法が書かれています。
  * <a href="http://book.cakephp.org/2.0/en/controllers.html#request-life-cycle-callbacks" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://book.cakephp.org/2.0/en/controllers.html#request-life-cycle-callbacks', 'Controllers — CakePHP Cookbook v2.x documentation']);" >Controllers — CakePHP Cookbook v2.x documentation</a>
  * <a href="http://api.cakephp.org/2.4/class-Controller.html#_invokeAction" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://api.cakephp.org/2.4/class-Controller.html#_invokeAction', 'CakeAPI > invokeAction']);" >CakeAPI > invokeAction</a>