# `vim-go` で `:GoDef` を使ってジャンプできなかった

## 事象

標準パッケージのメソッドなどを `:GoDef` すると以下のエラーメッセージが出る。

```
"vim-go: guru: no object for identifier"
```

関連issue:

https://github.com/fatih/vim-go/issues/1044

## 対応

上記 issue 内で完結してるけど、vim-go から `godef`, `guru` に委譲しているので、そちらをアップデートしてみる。

```
:GoUpdateBinaries
```
