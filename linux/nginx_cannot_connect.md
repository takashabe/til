# Nginxにつながらない

#### 問題

エラーログに以下のようなログが出力されている

```
2014/09/22 22:12:35 [crit] 1639#0: *4 connect() to 127.0.0.1:8080 failed (13: Permission denied) while connecting to upstream, client: 10.10.81.212, server: 10.10.81.82, request: "GET / HTTP/1.1", upstream: "http://127.0.0.1:8080/", host: "10.10.81.82"
```

#### 原因と対策

* selinuxで遮断されている場合
  * CentOS7でこの事象に遭遇した

```
setsebool -P httpd_can_network_connect 1
```

* パーミッションが無い
  * nginxを動かすユーザを変更した場合など

```
# 雑なやり方。対象のディレクトリは適宜変更する必要ありかも
chmod 766 -R /var/log/nginx
```
