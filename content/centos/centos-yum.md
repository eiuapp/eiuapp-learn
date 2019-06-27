---
title: "Centos Yum"
date: 2018-03-02T11:46:46+08:00
draft: false
tags: ["centos", "yum"]
categories: ["centos"]
subtitle: ""
descriptions: ""
bigimg:
---

centos系列之yum

## yum安装的软件在哪? linux下如何查看某个软件 是否安装？安装路径在哪?

[linux下如何查看某个软件 是否安装？安装路径在哪](https://zhidao.baidu.com/question/752680403805026404.html)

使用`sudo rpm -qa`来查看通过 yum安装的软件

    [tom@kube-node-11 ~]$ sudo rpm -qa | grep Maria
    MariaDB-client-10.1.19-1.el7.centos.x86_64
    MariaDB-common-10.1.19-1.el7.centos.x86_64
    MariaDB-Galera-server-10.0.28-1.el7.centos.x86_64
    MariaDB-shared-10.1.19-1.el7.centos.x86_64

## linux 通过 yum search 得到，在当前情况下，能安装哪些软件，哪些软件安装不了。

如下面 这个地方，显示 MariaDB-Galera-server 找不到。

    [cdn@test_240 ~]$ sudo yum install MariaDB-Galera-server MariaDB-client galera -y
    Loaded plugins: fastestmirror, langpacks
    Loading mirror speeds from cached hostfile
     * base: mirrors.cn99.com
     * extras: mirrors.cn99.com
     * updates: mirrors.cn99.com
    No package MariaDB-Galera-server available.
    Package MariaDB-client-10.1.19-1.el7.centos.x86_64 already installed and latest version
    Package galera-25.3.18-1.rhel7.el7.centos.x86_64 already installed and latest version
    Nothing to do
    [cdn@test_240 ~]$

那么，我们怎么办呢？
用`yum search`吧

1. 确保 MariaDB.repo 存在

    [cdn@test_240 ~]$ ll /etc/yum.repos.d/
    [cdn@test_240 ~]$ cat /etc/yum.repos.d/MariaDB.repo
    # MariaDB 10.1 CentOS repository list - created 2016-11-15 02:38 UTC
    # http://downloads.mariadb.org/mariadb/repositories/
    [mariadb]
    name = MariaDB
    baseurl = http://yum.mariadb.org/10.0/centos7-amd64
    gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
    gpgcheck=1
    [cdn@test_240 ~]$

2. 确保 MariaDB.repo 生效

先查是否能ping到baseurl

    [cdn@test_240 ~]$ cat /etc/yum.repos.d/MariaDB.repo
    # MariaDB 10.0 CentOS repository list - created 2016-11-16 02:27 UTC
    # http://downloads.mariadb.org/mariadb/repositories/
    [mariadb]
    name = MariaDB
    baseurl = http://yum.mariadb.org/10.0/centos7-amd64
    gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
    gpgcheck=1
    [cdn@test_240 ~]$ ping yum.mariabdb.org
    ^C
    [cdn@test_240 ~]$ ping yum.mariadb.org
    PING yum.mariadb.org (142.4.217.28) 56(84) bytes of data.
    64 bytes from bb02.mariadb.net (142.4.217.28): icmp_seq=1 ttl=47 time=272 ms
    64 bytes from bb02.mariadb.net (142.4.217.28): icmp_seq=2 ttl=47 time=272 ms
    64 bytes from bb02.mariadb.net (142.4.217.28): icmp_seq=3 ttl=47 time=272 ms
    ^C
    --- yum.mariadb.org ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2001ms
    rtt min/avg/max/mdev = 272.301/272.492/272.588/0.135 ms

说明可以`ping yum.mariadb.org`。

用 `yum search`查，有MariaDB.repo VS 没有MariaDB.repo情况下，得到的软件列表是否相同。

先走一个 `sudo yum search MariaDB`

    [cdn@test_240 ~]$ sudo yum search MariaDB
    Loaded plugins: fastestmirror, langpacks
    Loading mirror speeds from cached hostfile
     * base: mirrors.nwsuaf.edu.cn
     * epel: ftp.cuhk.edu.hk
     * extras: mirrors.cn99.com
     * updates: mirrors.cn99.com
    =============================================================================================== N/S matched: MariaDB ===============================================================================================
    MariaDB-client.x86_64 : MariaDB: a very fast and robust SQL database server
    MariaDB-common.x86_64 : MariaDB: a very fast and robust SQL database server
    MariaDB-compat.x86_64 : MariaDB: a very fast and robust SQL database server
    MariaDB-connect-engine.x86_64 : MariaDB: a very fast and robust SQL database server
    MariaDB-cracklib-password-check.x86_64 : MariaDB: a very fast and robust SQL database server
    MariaDB-devel.x86_64 : MariaDB: a very fast and robust SQL database server
    MariaDB-gssapi-client.x86_64 : MariaDB: a very fast and robust SQL database server
    MariaDB-gssapi-server.x86_64 : MariaDB: a very fast and robust SQL database server
    MariaDB-oqgraph-engine.x86_64 : MariaDB: a very fast and robust SQL database server
    MariaDB-server.x86_64 : MariaDB: a very fast and robust SQL database server
    MariaDB-shared.x86_64 : MariaDB: a very fast and robust SQL database server
    MariaDB-test.x86_64 : MariaDB: a very fast and robust SQL database server
    mariadb-bench.x86_64 : MariaDB benchmark scripts and data
    mariadb-devel.i686 : Files for development of MariaDB/MySQL applications
    mariadb-devel.x86_64 : Files for development of MariaDB/MySQL applications
    mariadb-embedded.i686 : MariaDB as an embeddable library
    mariadb-embedded.x86_64 : MariaDB as an embeddable library
    mariadb-embedded-devel.i686 : Development files for MariaDB as an embeddable library
    mariadb-embedded-devel.x86_64 : Development files for MariaDB as an embeddable library
    mariadb-libs.i686 : The shared libraries required for MariaDB/MySQL clients
    mariadb-libs.x86_64 : The shared libraries required for MariaDB/MySQL clients
    mariadb-server.x86_64 : The MariaDB server and related files
    mariadb.x86_64 : A community developed branch of MySQL
    mariadb-test.x86_64 : The test suite distributed with MariaD
    percona-xtrabackup.x86_64 : Online backup for InnoDB/XtraDB in MySQL, Percona Server and MariaDB

      Name and summary matches only, use "search all" for everything.
    [cdn@test_240 ~]$


两2种方法（2种用其1就可以了）：

1. 方法1, 把`MariaDB.repo`移除。
    不`MariaDB.repo` 移除，再运行 `sudo yum search MariaDB`。
    把`MariaDB.repo` 移除，再运行 `sudo yum search MariaDB`。

2. 方法2, 用`yum search --disablerepo=`。

    先 `sudo yum search MariaDB`。
    再运行 `sudo yum search MariaDB --disablerepo=base,extras,updates,epel`。

对比结果。最后，可知，列表不同。说明本`MariaDB.repo` 已生效。

*所以，以后每次安装软件前，可以先用`yum search`一下，这样，确保我们想要安装的，就是实际安装的*

## yum install 之前要做的事

2. sudo yum makecache
1. sudo yum clean all
3. sudo yum search XXX

## end
