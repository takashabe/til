# GKE

## ヘルスチェック

```yaml
readinessProbe:
    httpGet:
    path: /health-check
    port: 80
    scheme: HTTP
livenessProbe:
    httpGet:
    path: /healthz
    port: 80
    scheme: HTTP
```

* readinessProbe
    * いわゆる死活監視
    * レスポンスないと殺される
* livenessProbe
    * 依存関係含めたアプリケーションの起動監視
        * DBとかそういうやつも含めてOKになったら200返すようにする
    * レスポンスないとトラフィックが流れないようにされる

https://www.ianlewis.org/jp/kubernetes-health-check
