## CentOS 7でタイムゾーンを変更する

CentOS 7ではsystemdベースになっているため、6以前とはやり方が異なる。具体的には `timedatectl` を使用する。

#### 設定確認

```
$ timedatectl
      Local time: Fri 2016-02-19 06:58:27 UTC
  Universal time: Fri 2016-02-19 06:58:27 UTC
        RTC time: Fri 2016-02-19 06:58:26
       Time zone: Etc/UTC (UTC,  +0000)
     NTP enabled: yes
NTP synchronized: yes
 RTC in local TZ: no
      DST active: n/a
```

#### JSTに変更

```
$ sudo timedatectl set-timezone Asia/Tokyo

## 再度確認してみる
$ timedatectl
[isucon@ip-10-0-0-72 webapp]$ timedatectl
      Local time: Fri 2016-02-19 16:00:49 JST
  Universal time: Fri 2016-02-19 07:00:49 UTC
        RTC time: Fri 2016-02-19 07:00:49
       Time zone: Asia/Tokyo (JST,  +0900)
     NTP enabled: yes
NTP synchronized: yes
 RTC in local TZ: no
      DST active: n/a
```
