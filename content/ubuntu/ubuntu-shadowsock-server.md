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

事后, 经过参考 https://gist.github.com/icasdri/9b11744b3fc1c6c3b2d7730c68874530 发现,是的.

先给出正确的配置文件

```bash
root@utuntu:/home/ubuntu# cat /etc/systemd/system/shadowsocks.service
[Unit]
Description=Shadowsocks Client Service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/bin/python /usr/local/bin/sslocal -c /etc/shadowsocks.json -d start
ExecStart=/usr/bin/python /usr/local/bin/sslocal -c /etc/shadowsocks.json -d stop
PIDFile=/var/run/shadowsocks.pid
Restart=always
RestartSec=4

[Install]
WantedBy=multi-user.target
root@utuntu:/home/ubuntu#
```

此时, 状态是下面这样的:

```bash
root@utuntu:/home/ubuntu# sudo systemctl status shadowsocks
● shadowsocks.service - Shadowsocks Client Service
   Loaded: loaded (/etc/systemd/system/shadowsocks.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2019-07-17 13:06:33 CST; 3s ago
 Main PID: 27582 (python)
    Tasks: 1 (limit: 4915)
   CGroup: /system.slice/shadowsocks.service
           └─27582 /usr/bin/python /usr/local/bin/sslocal -c /etc/shadowsocks.json -d start

Jul 17 13:06:33 utuntu systemd[1]: Started Shadowsocks Client Service.
Jul 17 13:06:33 utuntu python[27551]: INFO: loading config from /etc/shadowsocks.json
Jul 17 13:06:33 utuntu python[27551]: 2019-07-17 13:06:33 WARNING  warning: local set to listen on 0.0.0.0, it's not safe
Jul 17 13:06:33 utuntu python[27551]: 2019-07-17 13:06:33 INFO     loading libcrypto from libcrypto.so.1.1
root@utuntu:/home/ubuntu#
```

### 开机自启动 ###

```bash
sudo systemctl enable shadowsocks
```

### (可跨过)配置过程 ###

如果少了 `PIDFile=/var/run/shadowsocks.pid` 这一句,

```bash
root@utuntu:/home/ubuntu# sudo systemctl status shadowsocks
● shadowsocks.service - Shadowsocks Client Service
   Loaded: loaded (/etc/systemd/system/shadowsocks.service; enabled; vendor preset: enabled)
   Active: activating (auto-restart) since Wed 2019-07-17 13:05:04 CST; 3s ago
  Process: 25389 ExecStart=/usr/bin/python /usr/local/bin/sslocal -c /etc/shadowsocks.json -d start (code=exited, status=0/SUCCESS)
 Main PID: 25389 (code=exited, status=0/SUCCESS)
root@utuntu:/home/ubuntu# telnet localhost 1080
Trying ::1...
Trying 127.0.0.1...
telnet: Unable to connect to remote host: Connection refused
root@utuntu:/home/ubuntu# telnet 127.0.0.1 1080
Trying 127.0.0.1...
telnet: Unable to connect to remote host: Connection refused
root@utuntu:/home/ubuntu# telnet 192.168.168.137 1080
Trying 192.168.168.137...
telnet: Unable to connect to remote host: Connection refused
root@utuntu:/home/ubuntu#
```

如果少了

```
PIDFile=/var/run/shadowsocks.pid
Restart=always
RestartSec=4
```

这3句, status 效果如下

```bash
root@utuntu:/home/ubuntu# sudo systemctl status shadowsocks
● shadowsocks.service - Shadowsocks Client Service
   Loaded: loaded (/etc/systemd/system/shadowsocks.service; enabled; vendor preset: enabled)
   Active: inactive (dead) since Mon 2019-07-15 09:47:27 CST; 2 days ago
  Process: 1254 ExecStart=/usr/bin/python /usr/local/bin/sslocal -c /etc/shadowsocks.json -d start (code=exited, status=0/SUCCESS)
 Main PID: 1254 (code=exited, status=0/SUCCESS)

Jul 15 09:47:26 utuntu systemd[1]: Started Shadowsocks Client Service.
Jul 15 09:47:27 utuntu python[1254]: INFO: loading config from /etc/shadowsocks.json
Jul 15 09:47:27 utuntu python[1254]: 2019-07-15 09:47:27 WARNING  warning: local set to listen on 0.0.0.0, it's not safe
Jul 15 09:47:27 utuntu python[1254]: 2019-07-15 09:47:27 INFO     loading libcrypto from libcrypto.so.1.1
root@utuntu:/home/ubuntu#
```








