---
title: "Mariadb Galera Panubo2 Base Docker"
date: 2018-03-02T11:26:51+08:00
draft: false
tags: ["mariadb", "panubo", "galera", "docker"]
categories: ["mariadb"]
subtitle: ""
descriptions: ""
bigimg:
---

docker之docker-mariadb-galera(panubo版)(2017-02)

## ref
- https://github.com/panubo/docker-mariadb-galera

## env

- node0, 10.10.13.110, primary
- node1, 10.10.15.240
- node2, 10.10.12.13(这一台暂时没用上)


## step

0. 拉取 (panubo版)git 代码, 取 image

三台机器, 各运行一下

    mkdir ~/docker && cd ~/docker && git clone https://github.com/panubo/docker-mariadb-galera && cd docker-mariadb-galera/ && docker pull panubo/mariadb-galera

1. 设置环境变量

在node0上:

    cat >> ~/.bashrc << EOF

    WSREP_NODE_ADDRESS=10.10.13.110
    WSREP_CLUSTER_ADDRESS=gcomm://10.10.13.110:4567,10.10.12.13:4567,10.10.15.240:4567
    WSREP_CLUSTER_NAME=my_wsrep_cluster
    WSREP_NODE_NAME=mariadb-node-0
    EOF

    source ~/.bashrc



在node1上:

    cat >> ~/.bashrc << EOF

    WSREP_NODE_ADDRESS=10.10.15.240
    WSREP_CLUSTER_ADDRESS=gcomm://10.10.13.110:4567,10.10.12.13:4567,10.10.15.240:4567
    WSREP_CLUSTER_NAME=my_wsrep_cluster
    WSREP_NODE_NAME=mariadb-node-1
    EOF

    source ~/.bashrc

在node2上:

    cat >> ~/.bashrc << EOF

    WSREP_NODE_ADDRESS=10.10.12.13
    WSREP_CLUSTER_ADDRESS=gcomm://10.10.13.110:4567,10.10.12.13:4567,10.10.15.240:4567
    WSREP_CLUSTER_NAME=my_wsrep_cluster
    WSREP_NODE_NAME=mariadb-node-2
    EOF

    source ~/.bashrc

2. 打开端口

在每一台机器上:

    sudo firewall-cmd --permanent --add-port=3306/tcp;
    sudo firewall-cmd --permanent --add-port=4567/udp;
    sudo firewall-cmd --permanent --add-port=4567/tcp;
    sudo firewall-cmd --permanent --add-port=4568/tcp;
    sudo firewall-cmd --permanent --add-port=4444/tcp;
    sudo firewall-cmd --reload;
    sudo firewall-cmd --permanent --list-port;

2. (这一步, 可以不做)Running Garbd

在 node0 上:

    docker run -d --net host --name galera-garbd \
    -e WSREP_CLUSTER_ADDRESS=$WSREP_CLUSTER_ADDRESS \
    panubo/mariadb-galera \
    garbd

3. 运行container, 创建新的数据库目录下

在 node0 上:

    sudo mkdir -p /mnt/data/galera.service/mysql/mysql

没有道理了哈, 下面这个成功了, 上面几个都没有成功..

    docker run -d --net host --name galera   -e WSREP_NODE_ADDRESS=$WSREP_NODE_ADDRESS   -e WSREP_CLUSTER_ADDRESS=$WSREP_CLUSTER_ADDRESS   -e MYSQL_ROOT_PASSWORD=like1123  -p 3306:3306   -p 4567:4567/udp   -p 4567-4568:4567-4568   -p 4444:4444   -v /mnt/data/galera.service/mysql:/var/lib/mysql:Z   panubo/mariadb-galera     mysqld     --wsrep-new-cluster


这里设置了 , mysql的root用户的密码为`like1123`

在node1上:

    sudo mkdir -p /mnt/data/galera.service/mysql/mysql

    docker run -d --net host --name galera -e WSREP_NODE_ADDRESS=$WSREP_NODE_ADDRESS \
    -e WSREP_CLUSTER_ADDRESS=$WSREP_CLUSTER_ADDRESS \
    -p 3306:3306 \
    -p 4567:4567/udp \
    -p 4567-4568:4567-4568 \
    -p 4444:4444 \
    -v /mnt/data/galera.service/mysql:/var/lib/mysql:Z \
    panubo/mariadb-galera \
    mysqld

4. 连接到 已有的数据库目录`/data/mariadb/mysql`


下面, 我们连接到 已有的数据库目录`/data/mariadb/mysql`:



准备工作, 每台机, 关闭, 删除container, 重新启动 docker.service :

    docker stop 3b8adb3825e1 && docker rm 3b8adb3825e1
    sudo rm -rf /mnt/data/galera.service/mysql
    sudo systemctl restart docker.service && sudo systemctl status docker.service

现在开始:


创建 数据库的数据目录, 如果有了, 就不用创建了, 只要把下面的目录修改成 数据目录 就可以了.

在node0上:

因为, 我们这里之前有过 数据库(但是, 不是docker下运行的), 数据库的数据目录为`/data/mariadb/mysql`, 所以这里直接用了


    docker run -d --net host --name galera   -e WSREP_NODE_ADDRESS=$WSREP_NODE_ADDRESS   -e WSREP_CLUSTER_ADDRESS=$WSREP_CLUSTER_ADDRESS   -e MYSQL_ROOT_PASSWORD=like1123  -p 3306:3306   -p 4567:4567/udp   -p 4567-4568:4567-4568   -p 4444:4444   -v /data/mariadb/mysql:/var/lib/mysql:Z   panubo/mariadb-galera mysqld --wsrep-new-cluster

在node1上:

    docker run -d --net host --name galera -e WSREP_NODE_ADDRESS=$WSREP_NODE_ADDRESS -e WSREP_CLUSTER_ADDRESS=$WSREP_CLUSTER_ADDRESS -p 3306:3306 -p 4567:4567/udp -p 4567-4568:4567-4568 -p 4444:4444 -v /data/mariadb/mysql:/var/lib/mysql:Z panubo/mariadb-galera mysqld

4. 添加 node2 进集群

确保一下, 数据库的数据目录为`/data/mariadb/mysql`

方式与 node1 一样, 直接添加, 就可以了.

在 node2 上:

    docker run -d --net host --name galera -e WSREP_NODE_ADDRESS=$WSREP_NODE_ADDRESS -e WSREP_CLUSTER_ADDRESS=$WSREP_CLUSTER_ADDRESS -p 3306:3306 -p 4567:4567/udp -p 4567-4568:4567-4568 -p 4444:4444 -v /data/mariadb/mysql:/var/lib/mysql:Z panubo/mariadb-galera mysqld


4. 验收:

验收1:

在 node0, 运行 `docker exec -ti node11-container-sha1 mysql -p -e 'show status like "wsrep_cluster_size"'`, 查看 集群的节点数.

    [tom@mariadb-node-1 ~]$ docker ps
    CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS               NAMES
    c0c6fa26b49a        panubo/mariadb-galera   "/galera-entrypoin..."   41 hours ago        Up 7 hours                              galera
    [tom@mariadb-node-1 ~]$ docker exec -ti c0c6fa26b49a mysql -p -e 'show status like "wsrep_cluster_size"'
    Enter password:
    +--------------------+-------+
    | Variable_name      | Value |
    +--------------------+-------+
    | wsrep_cluster_size | 2     |
    +--------------------+-------+
    [tom@mariadb-node-1 ~]$



验收2:

去 node0, 进入container, 新建立一个库,

    [tom@mariadb-node-0 ~]$ docker exec -it 1de01ad721ad /bin/bash
    root@mariadb-node-0:/# mysql -u root -p
    Enter password:
    Welcome to the MariaDB monitor.  Commands end with ; or \g.
    Your MariaDB connection id is 5
    Server version: 10.1.21-MariaDB-1~jessie mariadb.org binary distribution

    Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

    MariaDB [(none)]>

    CREATE DATABASE IF NOT EXISTS my_db default character set utf8 COLLATE utf8_general_ci;

去 node1, 进入container, 检查, 是否有

    show databases;

或者直接这样:

    container_sha1=c0c6fa26b49a # 这个是container sha1 字符串
    docker exec -ti $container_sha1 mysql -p -e 'CREATE DATABASE IF NOT EXISTS my_db default character set utf8 COLLATE utf8_general_ci;'
    docker exec -ti $container_sha1 mysql -p -e 'show databases;'


ok, 现在说明, galera已经建立好了.

5. 怎么让其它机器访问 galera

登陆其它机器

mysql -h 10.10.12.17 -u root -pmysql_root_password

这样, 就可以访问到 node0 上的mysql 啦.

6. Q&A

Q: 把 node1 的container `docker restart` , 是否 mysql 正常
A: 正常

Q: 把 node0 的container `docker restart` , 是否mysql 正常
A: container, 不正常. 重启后, 起不来了. 原因: 可能是因为, node0, 的安装过程中, 有`--wsrep-new-clusterr`参数, 重启, 则意味着, 在原来的文件夹上建立一个新的`--wsrep-new-clusterr`(相同的directory, 当然是不允许的), 所以报错.
如果不小心, 重启了 node0上的主container, 那怎么办?
* 方法1(适用于, 不要数据的情况)

先删除数据, 然后重新启动 docker.service, 就可以了.
* 方法2(适用于, 已有数据, 且要保留数据的情况)

TODO

6. 尝试 TODO

Q: 把端口号, 分别设置成`node0:3306, node1:4406`, `node0:4406, node1:3306`, `node0:4406, node1:4406`, 会有什么效果
A: TODO
