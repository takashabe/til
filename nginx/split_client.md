# split_client

## split_client とは

特定のトークンを与えると、それを任意の割合で指定した値を返してくれるモジュール

## sample

* やっていること
    * トークンとしてURLクエリパラメータの `token` を与えるようにしている
        * `${arg_xxx}` でクエリパラメータが取れる
        * 他にも `${remote_addr}` とかnginxで扱える値であれば使える
    * 0.5%の割合で `$version` に `edge` がバインドされる。残りは `stable` になる
    * `if` で `$version == edge` ならばリクエストパスに `/v2` を付与して `unix:/var/run/app.sock` にプロキシする

```nginx
http {
  upstream app {
    server unix:/var/run/app.sock;
  }

  split_clients "${arg_token}" $version {
    0.5%  edge;
    *     stable;
  }

  server {
    listen       80;
    server_name  localhost;

    location / {
      if ($version = edge) {
        rewrite /(.*) /v2/$1 break;
      }
      proxy_pass http://app;
    }
  }
}
```
