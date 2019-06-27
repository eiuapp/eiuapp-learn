---
title: "Ngrok Ssh"
date: 2018-04-23T00:05:45+08:00
draft: false
tags: ["ngrok", "ssh"]
categories: ["ngrok"]
subtitle: ""
descriptions: ""
bigimg:
---

远程登录家里的Ubuntu电脑(命令行模式)

## env


### 服务端
- 机器: 腾讯云主机一台
- IP: 111.230.153.251
- OS: ubuntu server 16.04.1
### 客户端

- 机器: 腾讯云主机一台
- IP: 192.168.31.106
- OS: ubuntu desktop 16.04.3

## step

### 服务端建立一个 ngrokd服务,开通 4445 tunnel

我这里写成一个服务了的.

```
ubuntu@VM-0-12-ubuntu:~$ cat /etc/systemd/system/ngrok-83-84-45.service
[Unit]
Description=ngrok
After=network.target

[Service]
ExecStart=/home/ubuntu/ngrok/ngrok-e/ngrok/bin/ngrokd -tlsKey=/home/ubuntu/ngrok/ngrok-e/ngrok/server.key -tlsCrt=/home/ubuntu/ngrok/ngrok-e/ngrok/server.crt -domain="hkshop.club" -httpAddr=":8083" -httpsAddr=":8084" -tunnelAddr=":4445"

[Install]
WantedBy=multi-user.target
ubuntu@VM-0-12-ubuntu:~$ 
ubuntu@VM-0-12-ubuntu:~$ sudo systemctl status ngrok-83-84-45.service
● ngrok-83-84-45.service - ngrok
   Loaded: loaded (/etc/systemd/system/ngrok-83-84-45.service; disabled; vendor preset: enabled)
   Active: active (running) since Sun 2018-04-22 09:05:30 CST; 24h ago
 Main PID: 20599 (ngrokd)
    Tasks: 5
   Memory: 2.3M
      CPU: 5.452s
   CGroup: /system.slice/ngrok-83-84-45.service
           └─20599 /home/ubuntu/ngrok/ngrok-e/ngrok/bin/ngrokd -tlsKey=/home/ubuntu/ngrok/ngrok-e/ngrok/server.key -tlsCrt=/home/ubuntu/ngrok/ngrok-e/ngrok/server.crt -domain=hkshop.club -httpAddr=:8083 -

Apr 23 09:40:00 VM-0-12-ubuntu ngrokd[20599]: [09:40:00 CST 2018/04/23] [INFO] (ngrok/log.(*PrefixLogger).Info:83) [metrics] Reporting: {"bytesIn.count":20501,"bytesOut.count":16763,"connMeter.count":6,"c
Apr 23 09:40:04 VM-0-12-ubuntu ngrokd[20599]: [09:40:04 CST 2018/04/23] [DEBG] (ngrok/log.(*PrefixLogger).Debug:79) [ctl:15ddd954] [753e7631548709e9649360ad9c3eb401] Reading message with length: 28
Apr 23 09:40:04 VM-0-12-ubuntu ngrokd[20599]: [09:40:04 CST 2018/04/23] [DEBG] (ngrok/log.(*PrefixLogger).Debug:79) [ctl:15ddd954] [753e7631548709e9649360ad9c3eb401] Read message {"Type":"Ping","Payload":
Apr 23 09:40:04 VM-0-12-ubuntu ngrokd[20599]: [09:40:04 CST 2018/04/23] [DEBG] (ngrok/log.(*PrefixLogger).Debug:79) [ctl:15ddd954] [753e7631548709e9649360ad9c3eb401] Waiting to read message
Apr 23 09:40:04 VM-0-12-ubuntu ngrokd[20599]: [09:40:04 CST 2018/04/23] [DEBG] (ngrok/log.(*PrefixLogger).Debug:79) [ctl:15ddd954] [753e7631548709e9649360ad9c3eb401] Writing message: {"Type":"Pong","Paylo
Apr 23 09:40:25 VM-0-12-ubuntu ngrokd[20599]: [09:40:25 CST 2018/04/23] [DEBG] (ngrok/log.(*PrefixLogger).Debug:79) [ctl:15ddd954] [753e7631548709e9649360ad9c3eb401] Reading message with length: 28
Apr 23 09:40:25 VM-0-12-ubuntu ngrokd[20599]: [09:40:25 CST 2018/04/23] [DEBG] (ngrok/log.(*PrefixLogger).Debug:79) [ctl:15ddd954] [753e7631548709e9649360ad9c3eb401] Read message {"Type":"Ping","Payload":
Apr 23 09:40:25 VM-0-12-ubuntu ngrokd[20599]: [09:40:25 CST 2018/04/23] [DEBG] (ngrok/log.(*PrefixLogger).Debug:79) [ctl:15ddd954] [753e7631548709e9649360ad9c3eb401] Waiting to read message
Apr 23 09:40:25 VM-0-12-ubuntu ngrokd[20599]: [09:40:25 CST 2018/04/23] [DEBG] (ngrok/log.(*PrefixLogger).Debug:79) [ctl:15ddd954] [753e7631548709e9649360ad9c3eb401] Writing message: {"Type":"Pong","Paylo
Apr 23 09:40:30 VM-0-12-ubuntu ngrokd[20599]: [09:40:30 CST 2018/04/23] [INFO] (ngrok/log.(*PrefixLogger).Info:83) [metrics] Reporting: {"bytesIn.count":20501,"bytesOut.count":16763,"connMeter.count":6,"c
lines 1-20/20 (END)
```

### 客户端

重点看一下 ngrok-4445-ssh-start.cfg 的配置

- server_addr 为 `domain name`:`tunnelAddr`
- remote_port 为 将来在外网连接时的 端口
- tcp 为客户端 ssh IP和端口, 如: "127.0.0.1:22" 

```
tom@udvbud-ngrokd-c:~/ngrok-cli$ pwd
/home/tom/ngrok-cli
tom@udvbud-ngrokd-c:~/ngrok-cli$ ls
ngrok  ngrok-4445-ssh.cfg  ngrok-4445-ssh-start.cfg  ngrok.cfg  ngrok.cfg.origin  ngrok-gitlab-111.cfg  nohup.out  README.md
tom@udvbud-ngrokd-c:~/ngrok-cli$ cat ngrok-4445-ssh-start.cfg
server_addr: "hkshop.club:4445"
trust_host_root_certs: false

tunnels:
    ssh:
       remote_port: 51001
       proto:
         tcp: "127.0.0.1:22"
tom@udvbud-ngrokd-c:~/ngrok-cli$
```

启动
```
tom@udvbud-ngrokd-c:~/ngrok-cli$ /home/tom/ngrok-cli/ngrok -config=ngrok-4445-ssh-start.cfg start ssh
```

### 服务端测试

通过 `ssh 客户端user@服务端IP -p客户端remeote_port` 的方式进行连接

```
ubuntu@VM-0-12-ubuntu:~/ngrok$ ssh tom@127.0.0.1 -p51001
tom@127.0.0.1's password:
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.13.0-38-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

206 packages can be updated.
0 updates are security updates.

Last login: Mon Apr 23 09:35:25 2018 from 127.0.0.1
tom@udvbud-ngrokd-c:~$
```
连接成功了

## ref

- [Ngrok远程桌面及ssh配置](https://www.lucktang.com/2557.html)
- [如何远程登录家里的Ubuntu电脑(命令行模式)](https://www.zhihu.com/question/27771692)