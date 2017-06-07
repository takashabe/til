## vim-easy-alignの使い方

使うたびにググっているのでメモっておく

### 基本の使い方:

1. 矩形選択
2. `ga` でvim-easy-align consoleに入る
3. `*` とalignのキーとなる記号を入力して `Enter`


### markdown tableの場合

`|` をキーとして並び替えてみる

* 原型

```
| name | content |
| --- | --- |
| hoge | foo |
```

* 整形

```
# `ga*|` で整形

| name | content |
| ---  | ---     |
| hoge | foo     |
```

#### Reference

* https://github.com/junegunn/vim-easy-align
