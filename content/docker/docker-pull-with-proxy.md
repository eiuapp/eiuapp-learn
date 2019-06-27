---
title: "Docker Pull With Proxy"
date: 2018-06-17T23:38:57+08:00
draft: false
tags: ["docker", "proxy"]
categories: ["docker"]
subtitle: "透过proxy进行docker pull"
descriptions: ""
bigimg:
---

透过proxy进行docker pull

## Env

- os: ubuntu16
- os: centos

## Step

- 对于Ubuntu 系统

```
sudo vi /etc/default/docker
```

- 对于Centos 系统

```
sudo vi /etc/sysconfig/docker
```

把下面的内容加到尾部

```
HTTP_PROXY=http://192.168.31.112:8118
http_proxy=${HTTP_PROXY}
HTTPS_PROXY=${HTTP_PROXY}
https_proxy=${HTTP_PROXY}
export HTTP_PROXY HTTPS_PROXY http_proxy https_proxy
```

重启docker

```
sudo systemctl restart docker
sudo systemctl status docker
```

然后再进行相关的 `docker pull` 操作就可以了。

## Ref

- https://blog.csdn.net/u011563903/article/details/52161648