# mysqld_safeについて

mysqldはmysqld_safeというmysqldの監視プログラムから起動される。
kill -9等でmysqldを終了した場合は自動起動される。そのためmysqld_safeからkillしなければならない。

https://dev.mysql.com/doc/refman/5.6/ja/mysqld-safe.html
