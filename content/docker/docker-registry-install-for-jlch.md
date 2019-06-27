---
title: "Docker Registry Install For Jlch"
date: 2018-03-02T11:30:46+08:00
draft: false
tags: ["docker", "registry", "install"]
categories: ["docker"]
subtitle: ""
descriptions: ""
bigimg:
---

docker之registry搭建(当然更建议使用Harbor)

## reference

- http://www.zimug.com/317.html?utm_source=tuicool&utm_medium=referral
- [docker-registry](http://www.zimug.com/317.html?utm_source=tuicool&utm_medium=referral)

 <!-- [1]:http://www.zimug.com/317.html?utm_source=tuicool&utm_medium=referral "docker-registry" -->

## env

docker版本大于1.6.0(`docker -v`)

机器3台:
* registry server , IP: 192.168.31.240
* registry client a(docker push), IP: 10.10.12.18
* registry client b(docker pull), IP: 10.10.13.10

## step

### 1. 创建registry server端

下载镜像

    docker pull registry:2

生成自签名证书


    cd ~/;mkdir registry && cd registry && mkdir certs && cd certs;openssl req -x509 -days 3650 -subj '/CN=reg.jlch.com/' -nodes -newkey rsa:2048 -keyout registry.key -out registry.crt;

生成用户和密码


    cd ~/registry&& mkdir auth;docker run --entrypoint htpasswd registry:2 -Bbn zimug zimug_password > auth/htpasswd;

用户：zimug 密码：zimug_password 可随便填写自己想填写的

启动registry server

脚本 `start_registry.sh` 放在~/registry目录下

    docker run -d -p 5000:5000 --restart=always --name registry \
    -v `pwd`/auth:/auth \
    -e "REGISTRY_AUTH=htpasswd" \
    -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
    -e "REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd" \
    -v `pwd`/certs:/certs \
    -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/registry.crt \
    -e REGISTRY_HTTP_TLS_KEY=/certs/registry.key \
    -v ~/data/registry2:/var/lib/registry \
    registry:2

确认registry server是UP状态，`docker ps -a | grep registry`


### 2. 配置docker server端

同registry server在同一台服务器上配置：

创建证书目录(没有此目录自己创建，注意端口号)

    sudo mkdir -p /etc/docker/certs.d/reg.jlch.com:5000
下载证书

    sudo cp ~/registry/certs/registry.crt /etc/docker/certs.d/reg.jlch.com:5000
域名解析,如果有DNS解析无需做此步骤（registry-server-ip=192.168.31.240）

    sudo echo 192.168.31.240 reg.jlch.com   >> /etc/hosts

### 3. 配置 `registry client a` 和 `registry client b`

其他主机(`registry client a` 和 `registry client b`)配置：

创建证书目录(没有此目录自己创建，注意端口号)

    sudo mkdir -p /etc/docker/certs.d/reg.jlch.com:5000
下载证书

    sudo scp -r tom@192.168.31.240:~/registry/certs/registry.crt /etc/docker/certs.d/reg.jlch.com:5000

域名解析,如果有DNS解析无需做此步骤（registry-server-ip=192.168.31.240）

    echo 192.168.31.240 reg.jlch.com >> /etc/hosts

验证测试

登陆(注意加端口号)

    docker login reg.jlch.com:5000
输入用户zimug，密码zimug_password以及邮箱

### 完成 `docker push`

更改镜像tag

    docker tag busybox reg.jlch.com:5000/busybox:1.0
push镜像

    docker push reg.jlch.com:5000/busybox:1.0

### 完成 `docker pull`

pull镜像

    docker pull reg.jlch.com:5000/busybox:1.0
