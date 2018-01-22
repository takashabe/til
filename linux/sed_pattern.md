# sed よく使うコマンド集

* 単純な置換

```shell
sed -i -e 's/package printserver/package display/g' *.go
```

* findと組み合わせる

```shell
find . -type f -name "*.go" -print0 | xargs -0 sed -i -e “s/package printserver/package display/
```
