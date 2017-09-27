# curlでcookie-jarを使う

curlでcookieを使う場合、cookieをファイルに書き出し、それを任意のリクエストで使う形になる。手動で任意のcookieを送ることも出来るがそれはここでは言及しない

* `-c, --cookie-jar`: cookieをファイルに書き出す
* `-b, --cookie`: ファイルをcookieとしてリクエストに付与する

よくあるログイン例

```shell
# ログイン
# -X POST -d ""... はpostと送信するデータを表す
curl -X POST -d "email=foo" -d "password=foo" http://localhost:8080/login

# ログインが必要な他のURL
curl -b cookie http://localhost:8080/
```
