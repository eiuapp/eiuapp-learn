---
title: "Backup Rancher"
date: 2018-06-19T00:18:49+08:00
draft: false
tags: ["rancher"]
categories: ["rancher"]
subtitle: "Backing Up Your Rancher Server"
descriptions: ""
bigimg:
---

Single Node Backup And Restoration

## Env

- 

## Step


backup rancher server

```
root@bitzone:~/.tom# docker stop e3655bea211c^C
root@bitzone:~/.tom# docker create --volumes-from e3655bea211c --name rancher-backup-v2.0.2 rancher/rancher:v2.0.2
464fd997687abe4cbc43fa39b30f4c246e620b6c33c8f032f32fdafebcd29c61
root@bitzone:~/.tom#
```

查看所有正在运行容器的hash-id
```
root@bitzone:~/.tom# docker ps -q
2211913af199
3cb7184995a4
5e50622bcc06
9e2549352d17
0287433011b0
2f7f9a341fcb
6b80dcdacaf5
d22d8ef3b283
a321ebd4941c
9c7e75ce47b3
0ea90d1c532f
1aec7af587f1
970a46ba0b57
698b46a9e3fb
fc9270fe5d50
d41ae6d7fa22
aab01d4e1556
95d8b9f51555
7a07fd5c94ce
a93266250acf
7b5c63b5de0e
dfda4227277f
84804443cd2c
e3655bea211c
root@bitzone:~/.tom#
```

重启rancher及容器
```
docker start 2211913af199
docker start 3cb7184995a4
docker start 5e50622bcc06
docker start 9e2549352d17
docker start 0287433011b0
docker start 2f7f9a341fcb
docker start 6b80dcdacaf5
docker start d22d8ef3b283
docker start a321ebd4941c
docker start 9c7e75ce47b3
docker start 0ea90d1c532f
docker start 1aec7af587f1
docker start 970a46ba0b57
docker start 698b46a9e3fb
docker start fc9270fe5d50
docker start d41ae6d7fa22
docker start aab01d4e1556
docker start 95d8b9f51555
docker start 7a07fd5c94ce
docker start a93266250acf
docker start 7b5c63b5de0e
docker start dfda4227277f
docker start 84804443cd2c
docker start e3655bea211c
```


docker 正常运行时
```
root@bitzone:~/.tom# docker ps | wc -l
29
root@bitzone:~/.tom#
root@bitzone:~/.tom# netstat tlpn | wc -l
416
root@bitzone:~/.tom# netstat -tlpn | wc -l
29
root@bitzone:~/.tom# netstat -tlpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:10248         0.0.0.0:*               LISTEN      5587/kubelet
tcp        0      0 127.0.0.1:10249         0.0.0.0:*               LISTEN      5567/kube-proxy
tcp        0      0 127.0.0.1:5001          0.0.0.0:*               LISTEN      23113/trackabled
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      5525/mysqld
tcp        0      0 172.31.158.93:56235     0.0.0.0:*               LISTEN      23113/trackabled
tcp        0      0 0.0.0.0:110             0.0.0.0:*               LISTEN      23861/dovecot
tcp        0      0 0.0.0.0:143             0.0.0.0:*               LISTEN      23861/dovecot
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      9551/apache2
tcp        0      0 127.0.0.1:6001          0.0.0.0:*               LISTEN      23113/trackabled
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      940/sshd
tcp        0      0 0.0.0.0:25              0.0.0.0:*               LISTEN      24447/master
tcp        0      0 0.0.0.0:1024            0.0.0.0:*               LISTEN      14836/python
tcp6       0      0 :::10250                :::*                    LISTEN      5587/kubelet
tcp6       0      0 :::9099                 :::*                    LISTEN      7335/calico-felix
tcp6       0      0 :::10251                :::*                    LISTEN      5483/kube-scheduler
tcp6       0      0 :::6443                 :::*                    LISTEN      5420/kube-apiserver
tcp6       0      0 :::2379                 :::*                    LISTEN      5446/etcd
tcp6       0      0 :::10252                :::*                    LISTEN      5378/kube-controlle
tcp6       0      0 :::2380                 :::*                    LISTEN      5446/etcd
tcp6       0      0 :::9900                 :::*                    LISTEN      25464/index.js
tcp6       0      0 :::110                  :::*                    LISTEN      23861/dovecot
tcp6       0      0 :::143                  :::*                    LISTEN      23861/dovecot
tcp6       0      0 :::10256                :::*                    LISTEN      5567/kube-proxy
tcp6       0      0 :::8088                 :::*                    LISTEN      5600/docker-proxy
tcp6       0      0 :::25                   :::*                    LISTEN      24447/master
tcp6       0      0 :::4443                 :::*                    LISTEN      5619/docker-proxy
tcp6       0      0 :::31102                :::*                    LISTEN      5567/kube-proxy
root@bitzone:~/.tom#
root@bitzone:~/.tom# docker ps | wc -l
29
root@bitzone:~/.tom# netstat -tlnp | wc -l
29
root@bitzone:~/.tom# ps -ef | wc -l
244
root@bitzone:~/.tom# ps -au | wc -l
11
root@bitzone:~/.tom# ps -ax | wc -l
244
root@bitzone:~/.tom#

```

---

现在stop docker
```
root@bitzone:~/.tom# systemctl stop docker
root@bitzone:~/.tom# netstat tlpn | wc -l
149
root@bitzone:~/.tom#
root@bitzone:~/.tom# netstat -tlpn | wc -l
16
root@bitzone:~/.tom# ps -ax | wc -l
174
root@bitzone:~/.tom# ps -ef | wc -l
174
root@bitzone:~/.tom# docker ps | wc -l
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
0
root@bitzone:~/.tom#
```
前后看得出，还是开了有很多进程的。

## Ref

- https://rancher.com/docs/rancher/v2.x/en/installation/backups-and-restoration/single-node-backup-and-restoration/
