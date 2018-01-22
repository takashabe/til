# rewrite

## rewrite とは

apacheの `mod_rewrite` 的なもの。正規表現でマッチしたクエリを任意のものに書き換える

## sample

* やっていること
    * conf内で定義された変数である `$version == edge` であれば、リクエストパスに `/v2` を挿入している
        * `example.com/hoge` -> `example.com/v2/hoge`
    * `/(.*)` でリクエストの `example.com/` 以降の文字列が取れる。そして `()` でグルーピングしているので `$1` で展開出来る
        * つまり基本的に他の正規表現エンジンと同じ

```nginx
location / {
    if ($version = edge) {
        rewrite /(.*) /v2/$1 break;
    }
    proxy_pass http://app;
}
```
