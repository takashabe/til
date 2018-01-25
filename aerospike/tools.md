# aerospike tools

## インストール時の注意点

必要なライブラリがないとのことで `aql` コマンドが失敗した

```shell
dyld: Library not loaded: /usr/local/lib/libcrypto.1.1.dylib
  Referenced from: /usr/local/bin/aql
  Reason: image not found
fish: 'aql' terminated by signal SIGABRT (Abort)
```

homebrew等で入れたopensslのライブラリをリンクする

```shell
ln -s /usr/local/Cellar/openssl@1.1/1.1.0g/lib/libssl.1.1.dylib /usr/local/lib/
ln -s /usr/local/Cellar/openssl@1.1/1.1.0g/lib/libcrypto.1.1.dylib /usr/local/lib/
```
