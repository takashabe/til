# nginx-ingress を使う

https://cloud.google.com/community/tutorials/nginx-ingress-gke

## 良さそうなチュートリアル

https://qiita.com/MahoTakara/items/cdfa379f2280a58fdd6d

- Caution:
    - RBACのサービスアカウント作成時、GKEの場合は以下をやってコマンドの実行アカウント？をcluster-adminにする必要がある
    - https://github.com/kubernetes/ingress-nginx/issues/1663#issuecomment-370448456

- Ingress:
    - 以下の違いは何？Hostヘッダの違いよくわかってない

```
imac:~ maho$ curl http://192.168.1.100/;echo
default backend - 404
```

```
imac:~ maho$ curl http://192.168.1.100/ -H 'Host: abc.sample.com'
<html><head><title>HTTP Hello World</title></head><body><h1>Hello from hello-world-deployment-b58876bbd-r6z86</h1></body></html
```
