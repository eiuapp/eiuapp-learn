---
title: "Ubuntu Network Proxy Shadowsocks Server"
date: 2018-06-11T20:34:39+08:00
draft: false
tags: ["network", "proxy", "shadowssocks"]
categories: ["proxy"]
subtitle: "ubuntu搭建SS客户端"
descriptions: ""
bigimg:
---

## Env

- os: ubuntu16
- ip: 192.168.168.137

## Ref

```bash
cd 
sudo pip install shadowsocks 
which sslocal
sudo vi /etc/shadowsocks.json
sslocal -c /etc/shadowsocks.json start
sslocal -c /etc/shadowsocks.json
sslocal -c /etc/shadowsocks.json -d start
vi /usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py
sslocal -c /etc/shadowsocks.json -d start
sslocal -c /etc/shadowsocks.json -d stop
sslocal -c /etc/shadowsocks.json -d start 
netstat -tlpnu
vi /etc/shadowsocks.json 
cat /etc/shadowsocks.json 
sslocal -c /etc/shadowsocks.json -d stop
sslocal -c /etc/shadowsocks.json -d start
sudo vim /etc/systemd/system/shadowsocks.service
sudo systemctl enable /etc/systemd/system/shadowsocks.service 
```

但是,发现,这个 shadowssocks.service 并不生效, 

```bash
ubuntu@utuntu:~$ sudo systemctl status shadowsocks
● shadowsocks.service - Shadowsocks Client Service
   Loaded: loaded (/etc/systemd/system/shadowsocks.service; enabled; vendor preset: enabled)
   Active: inactive (dead) since Thu 2019-06-13 15:34:15 CST; 4min 19s ago
  Process: 15037 ExecStart=/usr/local/bin/sslocal -c /etc/shadowsocks.json -d start (code=exited, status=0/SUCCESS)
 Main PID: 15037 (code=exited, status=0/SUCCESS)

Jun 13 15:34:15 utuntu systemd[1]: Started Shadowsocks Client Service.
Jun 13 15:34:15 utuntu sslocal[15037]: INFO: loading config from /etc/shadowsocks.json
Jun 13 15:34:15 utuntu sslocal[15037]: 2019-06-13 15:34:15 INFO     loading libcrypto from libcrypto.so.1.1
ubuntu@utuntu:~$ 
```

而且,只能通过root用户,运行

```bash
root@utuntu:/home/ubuntu# /usr/bin/python /usr/local/bin/sslocal -c /etc/shadowsocks.json -d start 
```
才能成功.这个是为什么呢?
是shadowsocks.service文件写得不对吗?




