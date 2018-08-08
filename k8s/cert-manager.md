# cert-manager と Let's Encrypt でSSL証明書を管理する

https://qiita.com/apstndb/items/3a39a1e6acacbbc30765

## Tipsまとめ

- IssuerとClusterIssuer
    - issuerはnamespaceに属する. 証明書の取得を行う
    - clusterIssuerはcluster全体に影響を与える
    - ちゃんとやるならissuerで細かくnamespace切ってやったほうが良さそう
- cert-managerとnamespace
    - cert-manager自体はhelmで`kube-system` namespaceあたりに入れてもOK
    - issuerは証明書を使うingressなどが属するnamespaceに作る
        - ingressから証明書を参照できるようにするため
    - certificateはissuerが取得した証明書を管理するためのCRD
        - これもingressからアクセスできるnamespaceに作る
- dns-01チャレンジ
    - dnsレコードにアクセスするためのsecret(サービスアカウント)を作れば管理が楽
        - GCPならcloud dnsへのアクセス権限が必要
    - これはissuerと同じnamespaceに作る必要がある
- namespaceまとめる
    - kube-system(どこでも良い)
        - cert-manager from helm
    - same as ingress
        - issuer
        - certificate
        - dnsチャレンジ用のサービスアカウント
