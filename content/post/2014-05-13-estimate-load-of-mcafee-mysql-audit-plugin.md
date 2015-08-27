---
title: McAfee MySQL Audit Plugin の負荷検証
author: 1000k
layout: post
date: 2014-05-13
url: /2014/05/13/estimate-load-of-mcafee-mysql-audit-plugin/
categories:
  - MySQL
tags:
  - MySQL
  - ベンチマーク
---
<a href="http://blog.1000k.net/?p=1837" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/?p=1837', '前の記事']);" >前の記事</a> で McAfee MySQL Audit Plugin を使うと簡単に監査ログが取れることを紹介しましたが、性能劣化はどれくらいあるのか心配になったので検証してみました。

<!--more-->

## Audit プラグインの設定値

以下の通り、ほぼインストールしたままの状態です。

```
mysql> SHOW VARIABLES LIKE 'audit_%';
+---------------------------------+----------------------------+
| Variable_name                   | Value                      |
+---------------------------------+----------------------------+
| audit_checksum                  |                            |
| audit_delay_cmds                |                            |
| audit_delay_ms                  | 0                          |
| audit_json_file                 | OFF                        | &lt;- Audit 有効時は ON にする
| audit_json_file_flush           | OFF                        |
| audit_json_file_sync            | 0                          |
| audit_json_log_file             | mysql-audit.json           |
| audit_json_socket               | OFF                        |
| audit_json_socket_name          | /tmp/mysql-audit.json.sock |
| audit_offsets                   |                            |
| audit_offsets_by_version        | ON                         |
| audit_record_cmds               |                            |
| audit_record_objs               |                            |
| audit_uninstall_plugin          | OFF                        |
| audit_validate_checksum         | ON                         |
| audit_validate_offsets_extended | ON                         |
| audit_whitelist_users           |                            |
+---------------------------------+----------------------------+
```


## ベンチマークの条件

mysqlslap なら簡単にベンチマークが取れます。詳しい使い方は <a href="http://blog.1000k.net/?p=1847" onclick="_gaq.push(['_trackEvent', 'outbound-article', 'http://blog.1000k.net/?p=1847', 'mysqlslap で MySQL の負荷テストをする方法']);" >mysqlslap で MySQL の負荷テストをする方法</a> をご覧ください。

```
mysqlslap \
  --no-defaults \
  --concurrency=100 \
  --iterations=5 \
  --number-int-cols=2 \
  --number-char-cols=3 \
  --engine=innodb \
  --auto-generate-sql \
  --auto-generate-sql-add-autoincrement \
  --auto-generate-sql-load-type=key \
  --auto-generate-sql-write-number=1000 \
  --number-of-queries=100000 \
  --host=localhost \
  --port=3306 \
  --user=root
```


## 結果

### Audit 無効時

```
Running for engine innodb
Average number of seconds to run all queries: 15.935 seconds
Minimum number of seconds to run all queries: 15.487 seconds
Maximum number of seconds to run all queries: 16.717 seconds
Number of clients running queries: 100
Average number of queries per client: 1000
```


### Audit 有効時

```
Running for engine innodb
Average number of seconds to run all queries: 19.083 seconds
Minimum number of seconds to run all queries: 18.763 seconds
Maximum number of seconds to run all queries: 19.902 seconds
Number of clients running queries: 100
Average number of queries per client: 1000
```


若干の性能劣化 (10%～20% 程度) が見られます。カツカツのシステムで使う場合は注意が必要かもしれません。