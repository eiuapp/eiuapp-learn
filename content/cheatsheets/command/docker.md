---
title: "Docker"
date: 2019-04-03T15:59:47+08:00
draft: false 
tags: ["docker", "command"]
categories: ["docker"]
subtitle: ""
descriptions: ""
bigimg:
---


## docker 删除所有容器，镜像，数据卷

```
#以下，按需求开启

#停止容器

docker stop $(docker ps -a -q)

#删除容器

docker rm $(docker ps -a -q)

#删除镜像

docker image rm $(docker image ls -a -q)

#删除数据卷：

#docker volume rm $(docker volume ls -q)

#删除 network：

#docker network rm $(docker network ls -q)


#----------------------------------------------------------------------------

#最直接并全面清理的的就是以下命令

#$docker stop $(docker ps -a -q) && docker system prune --all --force
```

## ref
- https://blog.csdn.net/qq_34924407/article/details/82777691
