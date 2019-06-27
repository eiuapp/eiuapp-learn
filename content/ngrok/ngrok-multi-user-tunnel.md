---
title: "Ngrok Multi User Tunnel"
date: 2018-04-22T09:02:38+08:00
draft: false
tags: ["ngrok"]
categories: ["ngrok"]
subtitle: ""
descriptions: ""
bigimg:
---

一个 ngrokd 服务配置多个协议


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

### 服务端

```
ubuntu@VM-0-12-ubuntu:~$ cat /etc/systemd/system/ngrok-e.service
[Unit]
Description=ngrok
After=network.target

[Service]
#/home/ubuntu/ngrok/ngrok-e/ngrok=/home/ubuntu/ngrok/ngrok-e/ngrok
ExecStart=/home/ubuntu/ngrok/ngrok-e/ngrok/bin/ngrokd -tlsKey=/home/ubuntu/ngrok/ngrok-e/ngrok/server.key -tlsCrt=/home/ubuntu/ngrok/ngrok-e/ngrok/server.crt -domain="hkshop.club" -httpAddr=":8081" -httpsAddr=":8082"

[Install]
WantedBy=multi-user.target
ubuntu@VM-0-12-ubuntu:~$ sudo systemctl status ngrok-e.service
● ngrok-e.service - ngrok
   Loaded: loaded (/etc/systemd/system/ngrok-e.service; disabled; vendor preset: enabled)
   Active: active (running) since Sat 2018-04-21 08:34:20 CST; 2 days ago
 Main PID: 19755 (ngrokd)
    Tasks: 5
   Memory: 2.6M
      CPU: 8.780s
   CGroup: /system.slice/ngrok-e.service
           └─19755 /home/ubuntu/ngrok/ngrok-e/ngrok/bin/ngrokd -tlsKey=/home/ubuntu/ngrok/ngrok-e/ngrok/server.key -tlsCrt=/home/ubuntu/ngrok/ngrok-e/ngrok/server.crt -domain=hkshop.club -httpAddr=:8081 -

Apr 23 10:01:21 VM-0-12-ubuntu ngrokd[19755]
```

### 客户端


#### 配置 yaml 文件

配置ngrok.cfg, 它是一个 yaml 文件

可以放到 [YAML在线格式化校验工具](https://www.bejson.com/validators/yaml/)测试一下, 经过格式化后,再粘贴到 ngrok.cfg 文件中

```
tom@udvbud-ngrokd-c:~/ngrok-cli$ cat ngrok.cfg
server_addr: "hkshop.club:4443"
trust_host_root_certs: false

tunnels:
    ssh:
        remote_port: 50022
        proto:
            tcp: "127.0.0.1:22"
    mstsc:
        remote_port: 53389
        proto:
            tcp: "127.0.0.1:3389"
    web:
        subdomain: "gitlab"
        proto:
            http: 8081
tom@udvbud-ngrokd-c:~/ngrok-cli$
```


启动

```
tom@udvbud-ngrokd-c:~/ngrok-cli$ ./ngrok -config=ngrok.cfg start ssh web
```

![ngrok-ssh-web](https://res.cloudinary.com/dmtixvmgt/image/upload/v1524449133/ngrok-multi-tunnels_feiqpq.jpg)

### 服务端连接测试

```
ubuntu@VM-0-12-ubuntu:~$ ssh tom@127.0.0.1 -p50022
tom@127.0.0.1's password:
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.13.0-38-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

206 packages can be updated.
0 updates are security updates.

Last login: Mon Apr 23 09:59:56 2018 from 127.0.0.1
tom@udvbud-ngrokd-c:~$
```

同时, 测试
`http://gitlab.hkshop.club:8081/`

可以正常打开

完成了一个 tunnelAdrr 下,同时做到ssh和http.

## ref

- [Ngrok远程桌面及ssh配置](https://www.lucktang.com/2557.html)
- [YAML在线格式化校验工具](https://www.bejson.com/validators/yaml/)
- [ngrok在Windows上内网穿透（网站+远程桌面连接）配置](https://blog.csdn.net/sinat_27938829/article/details/73088067)