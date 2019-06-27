+++
title = "用户加入docker组"
date = 2019-01-27T00:00:00-08:00
lastmod = 2019-01-27T19:20:34-08:00
tags = ["docker"]
categories = ["docker"]
draft = false
weight = 3001
+++

Docker 创建docker用户组，应用用户加入docker组


## 创建docker用户组 {#创建docker用户组}

```shell
sudo groupadd docker
```


## 应用用户加入docker用户组 {#应用用户加入docker用户组}

```shell
sudo usermod -aG docker ${USER}
```


## 重启docker服务 {#重启docker服务}

```shell
sudo systemctl restart docker
```


## 切换或者退出当前账户再从新登入 {#切换或者退出当前账户再从新登入}

```shell
su root           #  切换到root用户
su ${USER}        #  再切换到原来的应用用户以上配置才生效
```
