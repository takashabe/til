## ポートをどのプロセスが使用しているか調べる

#### プロセスからポートを調べる場合

* `lsof` コマンドを使用する

`lsof` コマンドはプロセスが開いているファイル(含ソケット)を出力するので、 `grep TCP` を噛ませてポートを使用しているものだけ抽出する。

```
### 基本形
# -n 名前解決を行わない
# -P ポート番号をポート名に変換しない
sudo lsof -nP | grep 'TCP'

### redisが使用しているポートを見る場合
sudo lsof -n -P | grep 'TCP' | grep 'redis'

### PIDが分かっている場合
# -p PIDを指定する
sudo lsof -n -P -p <pid> | grep 'TCP'
```

#### ポートからプロセスを調べる場合

* `lsof` コマンドを使う

```
# -i ポート番号を指定する
sudo lsof -P -i:<port>
```

* `netstat` コマンドを使用する

`-p` オプションでPIDが表示されるので、それを元に `ps -aux | grep <PID>` する。

_ただしBSD版の `netstat` では `-p` オプションがプロトコル指定になっており、PIDを調べることは出来ない模様_

```
# -a LISTEN 全ての接続を表示する
# -n 名前解決を行わない
# -p プロセスを表示する
sudo netstat -anp
```

---

参考

*http://blog.livedoor.jp/sonots/archives/32637678.html
