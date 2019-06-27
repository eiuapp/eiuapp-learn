---
title: "Docker Install Base Ubuntu"
date: 2018-06-04T19:01:35+08:00
draft: false
tags: ["ubuntu", "docker", "install"]
categories: ["docker"]
subtitle: "ubuntu中安装docker-ce"
descriptions: ""
bigimg:
---

ubuntu中安装docker-ce

## Env

- os: ubuntu16
- ip: *.*.*.*
- docker: 17.03.2~ce-0~ubuntu-xenial

## Step

### 查看已安装

```
sudo docker version
```

如果有已安装，请卸载，给个示例

```
sudo apt remove docker* -y
```

### 安装

```
lsb_release -a
sudo apt-get install     apt-transport-https     ca-certificates     curl     software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
apt-cache madison docker-ce
```
查看后，选择我们需要的指定版本, 我这里选择`17.03.2`.

```
sudo apt-get install docker-ce=17.03.2~ce-0~ubuntu-xenial -y
sudo docker run hello-world
```

## Ref

- https://docs.docker.com/install/linux/docker-ce/ubuntu/