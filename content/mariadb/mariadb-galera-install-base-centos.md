---
title: "Mariadb Galera Install Base Centos"
date: 2018-03-02T11:56:31+08:00
draft: false
tags: ["mariadb"]
categories: ["mariadb"]
subtitle: ""
descriptions: ""
bigimg:
---

mysql系列之集群 MariaDB-Galera-server 安装

MariaDB Galera Cluster 部署

## 参考
http://www.linuxidc.com/Linux/2015-07/119512.htm


## 安装


##### 安装mariadb10.0

>官方的[通过yum安装](https://mariadb.com/kb/zh-cn/installing-mariadb-with-yum/)教程

>根据不同的系统来下载repo吧，更多的[MariaDB.repo](https://downloads.mariadb.org/mariadb/repositories/#mirror=marty-anstey&distro=CentOS&distro_release=centos7-amd64--centos7&version=10.1)

或者，查看之前的 mariadb安装教程，就可以了。

具体，我的是这样的。
1. MariaDB.repo

    [cdn@test_240 yum.repos.d]$ cat /etc/yum.repos.d/MariaDB.repo
    # MariaDB 10.0 CentOS repository list - created 2016-11-16 02:27 UTC
    # http://downloads.mariadb.org/mariadb/repositories/
    [mariadb]
    name = MariaDB
    baseurl = http://yum.mariadb.org/10.0/centos7-amd64
    gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
    gpgcheck=1

### 2. 安装数据库

    sudo yum clean all
    sudo yum install MariaDB-Galera-server MariaDB-client galera -y
卸载
    sudo yum remove MariaDB-server MariaDB-client
    sudo yum remove MariaDB-common -y

如果要删除旧的数据库可以使用remove, 参数 -y 是确认，不用提示。此处，安装的是服务器和客户端，一般来说安装这两个就可以了。

### 3. 启动数据库
如果不用进行其他的操作，则现在就可以直接启动数据库，并进行测试了。

查看mysql状态;关闭数据库  
    # service mysql status
    # sudo /etc/init.d/mysql status
    # service mysql stop  
    # sudo /etc/init.d/mysql stop
启动数据库
    # service mysql start
    # sudo /etc/init.d/mysql start  
开机自启动
    # sudo chkconfig mysql on

*`sudo systemctl start mariadb.service`类似这种的方式，在这已失效*

    [cdn@test_240 ~]$ sudo systemctl start mariadb.service
    [sudo] password for cdn:
    Failed to start mariadb.service: Unit mariadb.service failed to load: No such file or directory.
    [cdn@test_240 ~]$ sudo /etc/init.d/mysql start
    Starting MySQL.161116 13:34:03 mysqld_safe Logging to '/var/lib/mysql/test_240.err'.
    .. SUCCESS!
    [cdn@test_240 ~]$ sudo chkconfig mysql on
    [cdn@test_240 ~]$ sudo /etc/init.d/mysql status
     SUCCESS! MySQL running (13959)
    [cdn@test_240 ~]$ sudo /etc/init.d/mysql stop
    Shutting down MySQL... SUCCESS!
    [cdn@test_240 ~]$

### 4. 修改root密码
    #  修改root密码  
    mysqladmin -u root password 'root'  
因为安装好以后的root密码是空,所以需要设置; 如果是测试服务器,那么你可以直接使用root,不重要的密码很多时候可以设置为和用户名一致，以免忘记了又想不起来。
如果是重要的服务器，请使用复杂密码，例如邮箱，各种自由组合的规则的字符等。

### 5. 登录数据库

    mysql -u root -p  
如果是本机,那可以直接使用上面的命令登录，当然，需要输入密码. 如果是其他机器，那么可能需要如下的形式:

    mysql -h 127.0.0.1 -P 3306 -u root -p  

### 6. 简单SQL测试

    >  
    -- 查看MySQL的状态
    status;  
    -- 显示支持的引擎
    show engines;  
    -- 显示所有数据库
    show databases;  
    -- 切换数据库上下文,即设置当前会话的默认数据库
    use test;  
    -- 显示本数据库所有的表
    show tables;  
    -- 创建一个表
    CREATETABLE t_test (  
      id int(11) UNSIGNED NOTNULL AUTO_INCREMENT,  
      userId char(36),  
      lastLoginTime timestamp,  
    PRIMARYKEY (id)  
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8;  
    -- 插入测试数据
    insertinto t_test(userId)  
    values
    ('admin')  
    ,('haha')  
    ;  
    -- 简单查询
    select * from t_test;  
    select id,userId from t_test  where userId='admin' ;  

### 7.  修改数据存放目录
mysql, MariaDB 的默认数据存放在 /var/lib/mysql/ 目录下,如果不想放到此处,或者是想要程序和数据分离，或者是磁盘原因,需要切换到其他路径,则可以通过修改 datadir系统变量来达成目的.

    # 停止数据库  
    service mysql stop
    # 创建目录,假设没有的话  
    mkdir /usr/local/ieternal/mysql_data  
    # 拷贝默认数据库到新的位置  
    # -a 命令是将文件属性一起拷贝,否则各种问题  
    cp -a /var/lib/mysql /usr/local/ieternal/mysql_data  
    # 备份原来的数据  
    cp -a /etc/my.cnf /etc/my.cnf_original  
    # 其实查看 /etc/my.cnf 文件可以发现  
    # MariaDB 的此文件之中只有一个包含语句  
    # 所以需要修改的配置文件为 /etc/my.cnf.d/server.cnf  
    cp /etc/my.cnf.d/server.cnf /etc/my.cnf.d/server.cnf_original  
    vim /etc/my.cnf.d/server.cnf    

然后 按 i 进入编辑模式,可以插入相关内容.使用键盘的上下左右键可以移动光标, 编辑完成以后，按 ESC 退出编辑模式(进入命令模式), 然后输入命令:wq 保存并退出

    # 在文件的 mysqld 节下添加内容  
    [mysqld]  
    datadir=/usr/local/ieternal/mysql_data/mysql  
    socket=/var/lib/mysql/mysql.sock  
    #default-character-set=utf8  
    character_set_server=utf8  
    slow_query_log=on
    slow_query_log_file=/usr/local/ieternal/mysql_data/slow_query_log.log  
    long_query_time=2  

其中,也只有 datadir 和 socket 比较重要; 而 default-character-set 是 mysql 自己认识的,而 mariadb5.5 就不认识,相当于变成了 character_set_server

##### 7.1 创建慢查询日志文件

既然上面指定了慢查询日志文件，我后来看了下MariaDB的err日志，发现MariaDB不会自己创建该文件，所以我们需要自己创建,并修改相应的文件权限(比如 MySQL 采用 mysql用户，可能我们使用 root用户创建的文件,此时要求慢查询日志文件对mysql用户可读可写就行。)

    touch /usr/local/ieternal/mysql_data/slow_query_log.log  
    chmod 666 /usr/local/ieternal/mysql_data/slow_query_log.log  

然后重新启动MySQL.

    service mysql start  

## 综合命令

方便大家ctrl+c, ctrl+v

    sudo service mysql status
    sudo service mysql stop
    cd /data/
    ls
    sudo mkdir ./mariadb/mysql/ -p
    ls
    cd mariadb/mysql/
    ls
    sudo cp -a /var/lib/mysql /data/mariadb/
    ls
    cd ../
    ls
    cd mysql/
    ls
    sudo cp -a /etc/my.cnf /etc/my.cnf_original
    sudo cp /etc/my.cnf.d/server.cnf /etc/my.cnf.d/server.cnf_original
    sudo vim /etc/my.cnf.d/server.cnf
    sudo touch /data/mariadb/mysql/slow_query_log.log
    sudo chmod 666 /data/mariadb/mysql/slow_query_log.log
    sudo systemctl restart mariadb.service

其中 vim /etc/my.cnf.d/server.cnf

    #
    # These groups are read by MariaDB server.
    # Use it for options that only the server (but not clients) should see
    #
    # See the examples of server my.cnf files in /usr/share/mysql/
    #

    # this is read by the standalone daemon and embedded servers
    [server]

    # this is only for the mysqld standalone daemon
    [mysqld]
    datadir=/data/mariadb/mysql  
    socket=/var/lib/mysql/mysql.sock  
    #default-character-set=utf8  
    character_set_server=utf8  
    slow_query_log=on  
    slow_query_log_file=/data/mariadb/mysql/slow_query_log.log  
    long_query_time=2


    # this is only for embedded server
    [embedded]

    # This group is only read by MariaDB-5.5 servers.
    # If you use the same .cnf file for MariaDB of different versions,
    # use this group for options that older servers don't understand
    [mysqld-5.5]

    # These two groups are only read by MariaDB servers, not by MySQL.
    # If you use the same .cnf file for MySQL and MariaDB,
    # you can put MariaDB-only options here
    [mariadb]

    [mariadb-5.5]

## 安装报错

### 报错1

报错：

    MariaDB-client-10.1.19-1.el7.centos.x86_64: [Errno 256] No more mirrors to try.

出错原因：

 之前已经在 /etc/yum.repos.d/MariaDB.repo 中设置过 `baseurl = http://yum.mariadb.org/10.1/centos6-amd64`, 而后又觉得不对，要降低为`baseurl = http://yum.mariadb.org/10.0/centos6-amd64`, 导致之前的缓存没有清除。

解决：

运行`sudo yum clean all`，然后再次运行`sudo yum install MariaDB-Galera-server MariaDB-client galera -y`

具体报错如下：

    [tom@kube-node-13 ~]$ ll /etc/yum.repos.d/
    总用量 44
    -rw-r--r--. 1 root root 1664 12月  9 2015 CentOS-Base.repo
    -rw-r--r--. 1 root root 1309 12月  9 2015 CentOS-CR.repo
    -rw-r--r--. 1 root root  649 12月  9 2015 CentOS-Debuginfo.repo
    -rw-r--r--. 1 root root  290 12月  9 2015 CentOS-fasttrack.repo
    -rw-r--r--. 1 root root  630 12月  9 2015 CentOS-Media.repo
    -rw-r--r--. 1 root root 1331 12月  9 2015 CentOS-Sources.repo
    -rw-r--r--. 1 root root 1952 12月  9 2015 CentOS-Vault.repo
    -rw-r--r--. 1 root root  957 3月  31 2016 epel.repo
    -rw-r--r--. 1 root root 1056 3月  31 2016 epel-testing.repo
    -rw-r--r--. 1 root root  261 11月 16 10:28 MariaDB.repo
    -rw-r--r--. 1 root root  401 2月  15 2016 zabbix.repo
    [tom@kube-node-13 ~]$ sudo yum install  MariaDB-client galera -y
    已加载插件：fastestmirror, langpacks
    Loading mirror speeds from cached hostfile
     * base: ftp.sjtu.edu.cn
     * epel: free.nchc.org.tw
     * extras: ftp.sjtu.edu.cn
     * updates: ftp.sjtu.edu.cn
    正在解决依赖关系
    --> 正在检查事务
    ---> 软件包 MariaDB-client.x86_64.0.10.1.19-1.el7.centos 将被 安装
    --> 正在处理依赖关系 MariaDB-common，它被软件包 MariaDB-client-10.1.19-1.el7.centos.x86_64 需要
    ---> 软件包 galera.x86_64.0.25.3.18-1.rhel7.el7.centos 将被 安装
    --> 正在处理依赖关系 libboost_program_options.so.1.53.0()(64bit)，它被软件包 galera-25.3.18-1.rhel7.el7.centos.x86_64 需要
    --> 正在检查事务
    ---> 软件包 MariaDB-common.x86_64.0.10.1.19-1.el7.centos 将被 安装
    ---> 软件包 boost-program-options.x86_64.0.1.53.0-25.el7 将被 安装
    --> 处理 MariaDB-common-10.1.19-1.el7.centos.x86_64 与 mariadb-libs < 1:10.1.19-1.el7.centos 的冲突
    --> 正在使用新的信息重新解决依赖关系
    --> 正在检查事务
    ---> 软件包 MariaDB-shared.x86_64.0.10.1.19-1.el7.centos 将被 舍弃
    ---> 软件包 mariadb-libs.x86_64.1.5.5.50-1.el7_2 将被 取代
    --> 解决依赖关系完成

    依赖关系解决

    ====================================================================================================================================================================================================================
     Package                                                  架构                                      版本                                                           源                                          大小
    ====================================================================================================================================================================================================================
    正在安装:
     MariaDB-client                                           x86_64                                    10.1.19-1.el7.centos                                           mariadb                                     39 M
     MariaDB-shared                                           x86_64                                    10.1.19-1.el7.centos                                           mariadb                                    1.3 M
          替换  mariadb-libs.x86_64 1:5.5.50-1.el7_2
     galera                                                   x86_64                                    25.3.18-1.rhel7.el7.centos                                     mariadb                                    7.8 M
    为依赖而安装:
     MariaDB-common                                           x86_64                                    10.1.19-1.el7.centos                                           mariadb                                     43 k
     boost-program-options                                    x86_64                                    1.53.0-25.el7                                                  base                                       155 k

    事务概要
    ====================================================================================================================================================================================================================
    安装  3 软件包 (+2 依赖软件包)

    总计：48 M
    总下载量：40 M
    Downloading packages:
    MariaDB-10.1.19-centos7-x86_64 FAILED                                          
    http://yum.mariadb.org/10.0/centos7-amd64/rpms/MariaDB-10.1.19-centos7-x86_64-client.rpm: [Errno 14] HTTP Error 404 - Not Found                                                   ]  0.0 B/s |    0 B  --:--:-- ETA
    正在尝试其它镜像。
    To address this issue please refer to the below knowledge base article

    https://access.redhat.com/articles/1320623

    If above article doesn't help to resolve this issue please create a bug on https://bugs.centos.org/

    MariaDB-10.1.19-centos7-x86_64 FAILED                                          
    http://yum.mariadb.org/10.0/centos7-amd64/rpms/MariaDB-10.1.19-centos7-x86_64-common.rpm: [Errno 14] HTTP Error 404 - Not Found                                                   ]  0.0 B/s |    0 B  --:--:-- ETA
    正在尝试其它镜像。
    MariaDB-10.1.19-centos7-x86_64 FAILED                                          
    http://yum.mariadb.org/10.0/centos7-amd64/rpms/MariaDB-10.1.19-centos7-x86_64-shared.rpm: [Errno 14] HTTP Error 404 - Not Found                                                   ]  0.0 B/s |    0 B  --:--:-- ETA
    正在尝试其它镜像。


    Error downloading packages:
      MariaDB-client-10.1.19-1.el7.centos.x86_64: [Errno 256] No more mirrors to try.
      MariaDB-shared-10.1.19-1.el7.centos.x86_64: [Errno 256] No more mirrors to try.
      MariaDB-common-10.1.19-1.el7.centos.x86_64: [Errno 256] No more mirrors to try.

    [tom@kube-node-13 ~]$

## end
