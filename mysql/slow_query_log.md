## スロークエリログ

クエリにどのくらい時間がかかったかなどの情報を出力するためのログ。プロファイリングツールはこのログを解析して結果を出しているものが多い。

#### 主要な設定項目

* `slow_query_log`
  * スロークエリログを出力するかどうか
* `long_query_time`
  * スロークエリログに出力するための閾値とする実行時間
  * 0 を設定することで全クエリが出力される
  * 0.5秒以上かかったクエリを出力したいのであれば `0.5` と指定する
* `log_queries_not_using_indexes`
  * インデックスが使われていないクエリを出力するかどうか
  * 有効化すると `long_query_time` に該当しないクエリでも出力されるようになる
* `slow_query_log_file`
  * スロークエリログの出力先
  * デフォルトでは `datadir/<hostname>-slow.log` に出力される
* `log_slow_admin_statements`
  * 管理ステートメントのログ出力を行うかどうか
  * 対象のステートメント
    * ALTER TABLE
    * ANALYZE TABLE
    * CHECK TABLE
    * CREATE INDEX
    * DROP INDEX
    * OPTIMIZE TABLE
    * REPAIR TABLE

#### 設定反映

上述の設定値を `my.cnf` に書く。もしくはmysqlコンソール上で `set global` することで再起動無しに設定の有効無効を切り替えることが可能。

設定ファイルに書く

```
$ cat /etc/my.cnf
[mysqld]
slow_query_log=1
long_query_time=1
```

一時的に有効にする

```
mysql> set global slow_query_log=1;
mysql> set global long_query_time=1;
```
