---
title: fixtureをディレクトリに分けて管理する
author: 1000k
layout: post
date: 2011-07-05
url: /2011/07/05/mange-fixtures-in-several-directories/
categories:
  - CakePHP
  - PHP
tags:
  - CakePHP
  - PHP
  - TIPS
  - 改造
---
**(2011/07/15 Update) 7/13以前のコードに誤りがあり、正しく動作しなかったので修正しました。**

## やりたいこと

CakePHPの規則に従うと、フィクスチャは /app/test/fixtures/ ディレクトリにすべて入れなければなりません。この場合、「同じモデルを使うけど、テストケース毎に異なるフィクスチャを代入したい」という要望をかなえるのが難しいです。

例えば、

  * Hige コントローラー
  * Moja コントローラー

どちらも同じ Uso モデルを使う場合を考えます。

それぞれで違うフィクスチャを使いたくても、Uso モデルのフィクスチャは uso_fixture.php の1個です。フィクスチャの値を両方のテストケースで使えるようにするのは難しく、まして複数人でテスト設計するのは困難です。

そこで、それぞれのコントローラーから呼び出すフィクスチャを

  * /app/test/fixtures/hige/uso_fixture.php
  * /app/test/fixtures/moja/uso_fixture.php

と使い分けられるようにする方法を考えました。

<!--more-->

## やり方

CakeTestCase を拡張した MyCakeTestCase を作成し、その中で _loadFixtures をオーバーライドします。

なお、参考にしたバージョンは CakePHP 1.3.10 です。

**/app/libs/my\_cake\_test_case.php**

```php
<?php
/**
 * Extension of CakeTestCase Class
 *
 * @author  $Author: senge.keiyo $
 * @version $id$
 */
class MyCakeTestCase extends CakeTestCase {

    /**
     * Load fixtures specified in var $fixtures.
     *
     * fixtureディレクトリ配下の任意のディレクトリにフィクスチャを配置できるよう拡張。
     *
     * 例えば $fixtures に 'app.unit_test.uso' と指定すると、
     * /app/tests/fixtures/unit_test/uso_fixture.php をロードできる。
     *
     * @author senda.keijiro
     * @return void
     * @access protected
     */
    function _loadFixtures() {
        if (!isset($this->fixtures) || empty($this->fixtures)) {
            return;
        }

        if (!is_array($this->fixtures)) {
            $this->fixtures = array_map('trim', explode(',', $this->fixtures));
        }

        $this->_fixtures = array();

        foreach ($this->fixtures as $index => $fixture) {
            $fixtureFile = null;
            $fixturePaths = null;

            if (strpos($fixture, 'core.') === 0) {
                $fixture = substr($fixture, strlen('core.'));
                foreach (App::core('cake') as $key => $path) {
                    $fixturePaths[] = $path . 'tests' . DS . 'fixtures';
                }
            } elseif (strpos($fixture, 'app.') === 0) {
                // MODIFIED
                // app.unittest.plan が来たら /fixtures/unittest/plan_fixtures.php
                // をロードするようにする
                $parts = explode('.', $fixture);
                $fixture = $parts[count($parts) - 1];

                array_shift($parts);
                array_pop($parts);
                $path = implode(DS, $parts);

                $fixturePaths = array(
                    TESTS . 'fixtures' . DS . $path,
                    TESTS . 'fixtures',
                    VENDORS . 'tests' . DS . 'fixtures'
                );
            } elseif (strpos($fixture, 'plugin.') === 0) {
                $parts = explode('.', $fixture, 3);
                $pluginName = $parts[1];
                $fixture = $parts[2];
                $fixturePaths = array(
                    App::pluginPath($pluginName) . 'tests' . DS . 'fixtures',
                    TESTS . 'fixtures',
                    VENDORS . 'tests' . DS . 'fixtures'
                );
            } else {
                $fixturePaths = array(
                    TESTS . 'fixtures',
                    VENDORS . 'tests' . DS . 'fixtures',
                    TEST_CAKE_CORE_INCLUDE_PATH . DS . 'cake' . DS . 'tests' . DS . 'fixtures'
                );
            }

            foreach ($fixturePaths as $path) {
                if (is_readable($path . DS . $fixture . '_fixture.php')) {
                    $fixtureFile = $path . DS . $fixture . '_fixture.php';
                    break;
                }
            }

            if (isset($fixtureFile)) {
                require_once($fixtureFile);
                $fixtureClass = Inflector::camelize($fixture) . 'Fixture';
                $this->_fixtures[$this->fixtures[$index]] =& new $fixtureClass($this->db);
                $this->_fixtureClassMap[Inflector::camelize($fixture)] = $this->fixtures[$index];
            }
        }

        if (empty($this->_fixtures)) {
            unset($this->_fixtures);
        }
    }
}
```


あとはテストケースでこのクラスを呼び出してやればOKです。

\*\*/app/tests/cases/controllers/higes_controller.test.php\*\*

```php
<?php
App::import('Model', 'Uso');
App::import('Lib', 'MyCakeTestCase');

class HigesControllerTestCase extends MyCakeTestCase {
    var $fixtures = array(
        'app.hige.uso'      // /app/test/fixtures/hige/uso_fixture.php がロードされる
    );
?>
```


## 副作用

テストケースで「App::import('Lib', 'MyCakeTestCase');」すると、なぜかMyCakeTestCaseもテスト対象になるらしく、テスト結果の「n/n test cases complete」の値が1増えます。

テスト結果のGreen/Redには影響しないので、無視してください。

## 参考

  * [App::import() は凄い - 24時間CakePHP](http://d.hatena.ne.jp/hiromi2424/20101215/1292379625)
      * 本当はApp::importだけでできれば一番いいんですけどね。