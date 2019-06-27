---
title: "Yapi Transfer Base Docker"
date: 2018-06-28T18:37:43+08:00
draft: false
tags: ["yapi"]
categories: ["yapi"]
subtitle: "记一次yapi的迁移过程"
descriptions: ""
bigimg:
---

记一次yapi的迁移过程

## Env

原来有一个docker安装在 192.168.168.162. 现在希望迁移至 192.168.168.137 

原来 docker 的安装方式,见, https://github.com/eiuapp/docker-yapi

同时也可以参考一下 https://www.cnblogs.com/woshimrf/p/docker-install-yapi.html

## Step

### 安装文件与容器数据迁移
```bash
tar -zcvf yapi.tar.gz ./yapi/
tar zcvf ./data.opt.mongodb.data.tar.gz /data/opt/mongodb/data/
scp -P 2222 data.opt.mongodb.data.tar.gz yapi.tar.gz ubuntu@192.168.168.137:/home/ubuntu/
```

### 新机器安装docker网络与相关镜像

```bash
tar -zxvf yapi.tar.gz
tar -zxvf data.opt.mongodb.data.tar.gz
sudo mv data/opt/ /data/opt/
ls /data/opt/mongodb/data/
cd yapi/
cp index.sh rindex.sh
vi rindex.sh
```

修改一下`rindex.sh`文件

```bash
ubuntu@utuntu:~/yapi$ cat rindex.sh
#!/bin/bash

#git clone https://github.com/Ryan-Miao/docker-yapi.git

cd docker-yapi
bash build.sh 1.5.10
bash start.sh  init-network
bash start.sh start-mongo
#bash start.sh init-mongo
#bash start.sh init-yapi
#bash start.sh logs-yapi
ubuntu@utuntu:~/yapi$ ./rindex.sh
```

验证一下, 这个环境可以了.

- docker images: mongo, yapi应该已准备好
- docker network: 有 tools-net 
- docker container: 有 mongod 

```bash
ubuntu@utuntu:~/yapi$ docker images | grep mongo
mongo                          4                   0fb47b43df19        2 weeks ago         411MB
ubuntu@utuntu:~/yapi$ docker images | grep yapi
yapi                           1.5.10              ece770eae458        7 minutes ago       153MB
yapi                           latest              ece770eae458        7 minutes ago       153MB
ubuntu@utuntu:~/yapi$ docker network ls | grep tools-net
6172fe3cf9c6        tools-net           bridge              local
ubuntu@utuntu:~/yapi$ docker ps | grep mongo
104964ede6a7        mongo:4                                "docker-entrypoint.s…"   10 minutes ago      Up 10 minutes           0.0.0.0:27017->27017/tcp                                         mongod
ubuntu@utuntu:~/yapi$
```

启动 yapi

```bash
ubuntu@utuntu:~/yapi/docker-yapi$ sudo docker run -d -p 3001:3001 --name yapi --net tools-net --ip 172.18.0.3 yapi
63a6152d324b9f5b5dfda40923cdf35dcf8580bc38090eed4b00ebfdf065a1e5
ubuntu@utuntu:~/yapi/docker-yapi$ sudo  docker logs --tail 10 yapi
log: 服务已启动，请打开下面链接访问:
http://127.0.0.1:3001/
log: mongodb load success...
ubuntu@utuntu:~/yapi/docker-yapi$
```

开启防火墙, 应该就可以访问了.






