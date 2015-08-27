---
title: '[CodeIgniter] Modelの使い方'
author: 1000k
layout: post
date: 2010-05-24
url: /2010/05/24/usage-of-codeigniter-model/
categories:
  - CodeIgniter
  - PHP
tags:
  - CodeIgniter
  - PHP
  - チュートリアル
---
# Modelの役割

  * ControllerやViewから独立して、DBのデータの取り扱い(CRUD)だけに専念する
  * CodeIgniterでは、Modelクラスを継承することで、直感的なDB操作メソッドが利用できる

# 簡単な例

下記は「Usersテーブルのデータを取得したり件数をカウントしたりするクラス」の例。 **/system/application/models/users_model.php**とする。

```
<?php
class Users_model extends Model {

    function __construct() {
        // ※コンストラクタでは必ず親クラスを継承する
        parent::Model();

        // database.php で定義したDBに接続する
        $this->load->database();
    }

    function getAllUsers() {
        $query = $this->db->get("users");

        if($query->num_rows() > 0) {
            // 結果セットを連想配列として返す
            return $query->result_array();
        }
    }

    function getUsersWhere($field, $param) {
        $this->db->where($field, $param);
        $query = $this->db->get("users");
        // 結果セットを連想配列として返す
        return $ruery->result_array();
    }

    function getNumUsers() {
        return $this->db->count_all("users");
    }

}
/** End of PHP file */
```


データベースクラスの細かい実装は <a href="http://codeigniter.jp/user_guide_ja/database/index.html" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://codeigniter.jp/user_guide_ja/database/index.html', 'データベースクラス : CodeIgniter ユーザガイド 日本語版']);" title="データベースクラス : CodeIgniter ユーザガイド 日本語版">データベースクラス : CodeIgniter ユーザガイド 日本語版</a> を参照。以下に挙げるものはよく使うので覚えておいた方が良い。

| クエリ                                              | 意味                                           |
| ------------------------------------------------ | -------------------------------------------- |
| $this->db->get(&#8216;table_name&#8217;)         | table_nameテーブル内の全行を取得する                      |
| $this->db->where($field,$param)                  | SQLのWHERE句の要領で行を取得する (WHERE $field = $param) |
| $this->db->count\_all(&#8216;table\_name&#8217;) | table_nameテーブルの行数を取得する                       |
| $query->result_array()                           | 結果セット(result set)を連想配列として返す                  |
| $query->num_rows()                               | 結果セットの行数を返す                                  |

このModelクラスのAPIを使うことで、Controllerから簡単にDBのデータを取得できる。

# ControllerからModelのAPIを呼び出す

**/system/application/controllers/Users.php** を下記のようにすることで、上述のモデルで作成したAPIをロードできる。

```
<?php
class Users extends Controller {

    function __construct() {
        // ※コンストラクタ内では親クラスをロードするお約束
        parent::Controller();
        // Usersモデルをロードする
        $this->load->model("users_model");
    }

    function index() {
        // Usersモデルで先ほど作成したAPIを利用する
        $data["users"] = $this->user_model->getAllUsers();
        $data["numusers"] = $this->user_model->getNumUsers();

        $this->load->view("users_view", $data);
    }
}
/** End of PHP file */
```


# 参考

<a href="http://www.devshed.com/c/a/PHP/Building-a-DatabaseDriven-Application-with-the-Code-Igniter-PHP-Framework/" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://www.devshed.com/c/a/PHP/Building-a-DatabaseDriven-Application-with-the-Code-Igniter-PHP-Framework/', 'Building a Database-Driven Application with the Code Igniter PHP Framework']);" title="Building a Database-Driven Application with the Code Igniter PHP Framework">Building a Database-Driven Application with the Code Igniter PHP Framework</a>

（文字がクレイジーなほど小さくて読みづらいので、「PDF Version Of Article」を見るのがおすすめ）