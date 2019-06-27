---
title: "Docker Registry Mirrors Base Ubuntu"
date: 2018-06-22T16:38:30+08:00
draft: false
tags: ["docker", "mirrors"]
categories: ["docker"]
subtitle: "如何配置镜像加速器"
descriptions: ""
bigimg:
---

如何配置镜像加速器

## Env

- docker: 17.03.2+
- os: ubuntu16.04

## Step

打开 https://cr.console.aliyun.com/#/imageList 找到 `镜像加速器`

可以根据自己OS选择，下面的是ubuntu, registry-mirrors中的内容已失效，请替换成您自己的。

针对Docker客户端版本大于1.10.0的用户
您可以通过修改daemon配置文件`/etc/docker/daemon.json`来使用加速器：

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://0d6wdn2yzz.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## Ref

- https://cr.console.aliyun.com/?spm=a2c4e.11153940.blogcont29941.9.520269d6m3p5Xn&accounttraceid=1843b6a4-8ded-4ec5-bdd5-fc17fcffbf8f&accounttraceid=93aba21f-da4b-41b2-ba82-c692f6d5c65f#/accelerator
- https://cr.console.aliyun.com/#/imageList