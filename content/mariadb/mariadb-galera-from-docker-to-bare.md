---
title: "Mariadb Galera From Docker to Bare"
date: 2018-03-02T12:49:14+08:00
draft: false
tags: ["mariadb", "galera", "docker"]
categories: ["mariadb"]
subtitle: ""
descriptions: ""
bigimg:
---

mysql系列之 从 docker+Galera 转到 Galera(去除 docker)


## context

之前在 [docker之docker-mariadb-galera(panubo版)](http://tomtsang.github.io/2017/01/24/docker-3/) 中成功在 docker 中安装了 Galera, 突然, 由于业务调整, 需要把 Galera 从docker中脱离出来, 直接在 裸机 上跑..


node0, 10.10.13.110, primary
node1, 10.10.15.240
node2, 10.10.12.13(这一台暂时没用上)

## step


1. [在node0, node1, node2上]把 Galera 从 docker 中删除

    docker stop abcd

2. [node0上]然后, 我尝试重新启动 MariaDB-Galera-cluster, 怎么重启, 那自然是参考 [mysql系列之集群 MariaDB Galera Cluster 部署](http://tomtsang.github.io/2016/11/17/mysql-4mysql%E7%B3%BB%E5%88%97%E4%B9%8B%E9%9B%86%E7%BE%A4%20MariaDB%20Galera%20Cluster%20%E9%83%A8%E7%BD%B2/)

来到 node0上,

    sudo /etc/init.d/mysql status
    sudo /etc/init.d/mysql start --wsrep-new-cluster

发现, 直接报错了..

    [tom@mariadb-node-0 ~]$ sudo /etc/init.d/mysql start --wsrep-new-cluster
    Starting MySQL.170317 13:22:10 mysqld_safe Logging to '/data/mariadb/mysql/mariadb-node-0.err'.
     ERROR!
    [tom@mariadb-node-0 ~]$

这下可好了, 怎么办呢?

去调试吧...

3. 调试, 使用 `--verbose`

    sudo -u mysql mysqld --verbose --defaults-file=/etc/my.cnf.d/server.cnf
去查一下, 发现, 原来是权限不对..为什么会权限不对? 噢, 原来是因为, 之前是在 docker 下安装的 galera, 那自然, 下面的这些数据目录(/data/mariadb/mysql/),是属于 docker 的所有者(panubo)的啦.然后现在要直接在机器上安装galera,那自然这些数据目录(/data/mariadb/mysql/)应该要属于 mysql:mysql才对, 那意味着, 用户组不对, 自然没权限, 自然报错啦...

怎么搞? 修改权限啦!

    sudo chown -R mysql:mysql /data/mariadb/mysql/
好了, 重新启动galera集群
    sudo /etc/init.d/mysql start --wsrep-new-cluster

哈哈, 成功启动 node0 了..

3. 把 node1, node2, 也同样地, 修改 数据目录 的所有者权限..重启mysql吧..

    [tom@mariadb-node-2 ~]$ sudo /etc/init.d/mysql start
    Starting MySQL.170317 16:54:53 mysqld_safe Logging to '/data/mariadb/mysql/mariadb-node-2.err'.
    .... SUCCESS!
    [tom@mariadb-node-2 ~]$

哈哈, 成功启动 node1, node2..

4. 那检查一下, galera, 正常了没有...

    [tom@test_240 ~]$ mysql -u root -p -e "show status like 'wsrep%';"
    Enter password:
    +------------------------------+------------------------------------------------------+
    | Variable_name                | Value                                                |
    +------------------------------+------------------------------------------------------+
    | wsrep_local_state_uuid       | 825cf372-abf0-11e6-9f33-b3514b1b22d5                 |
    ...
    | wsrep_cluster_conf_id        | 3                                                    |
    | wsrep_cluster_size           | 3
    ...

哈哈, 说明, 我们的 galera , 真的成功启动了...

## end
