# systemdでデータディレクトリとか設定ファイルの場所を変更したい

## Motivation

ISUCONなどでサーバをいじるとき、ミドルウェアの設定ファイルなどはgitで管理したい。
大抵モノリシックなリポジトリを作って、そこからsymbolic linkを貼ることが多い。そこでaptで入れたRedisが設定ファイルを読めずに起動出来ない事象に遭遇した。

## ProtectSystem, ProtectHome

昨今SELinuxでハマることは少なくなってきた気がするが、Protect*は知らなかったのでハマった。

- `ProtectSystem=full` => `ProtectSystem=off`
    - /usr とか /etc に書き込み出来なくなる
- `ProtectHome=true` => `ProtectHome=false`
    - /home, /root, /run/user に読み書き出来なくなる
