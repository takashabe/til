# k8s関連メモ

## ingress

- HTTP(S)ロードバランサ
    - external IP addressを持つ
    - serviceの前段に配置するLB

## deployment

- podの状態を定義する
- レプリカ数とかcontainer, imageの指定など

## service

- podの集合を一つのサービスとして管理する
- deploymentなどで定義したlabelに対してトラフィックを流す事ができる
    - pod自体はIPアドレスが変動するため、その前段でDNS(kube-dns)を使う必要があり、その単位がserviceになる
    - `kubectl get svc` でIPアドレスを持つことが分かる
- TCPロードバランサの役目を持つことも出来る
    - kind: LoadBalancer
