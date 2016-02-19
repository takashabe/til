## SELinuxの無効化

#### 一時的に無効化する

```
# 状態確認
$ getenforce
Enforcing # 有効化されている

# 一時的に無効化する
$ sudo setenforce 0
$ getenforce
Permissive # 無効化されている

[root@centos ~]# vi /etc/sysconfig/selinux　←　SELinux設定ファイル編集
SELINUX=enforcing
↓
SELINUX=disabled　←　システム起動時にSELinuxを無効化
```

#### システム起動時に無効化する

`/etc/sysconfig/selinux` を以下のように編集する

```
- SELINUX=enforcing
+ SELINUX=disabled
```

※ 即座に反映されないので注意
