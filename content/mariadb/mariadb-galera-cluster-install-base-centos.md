---
title: "Mariadb Galera Cluster Install Base Centos"
date: 2018-03-02T12:43:16+08:00
draft: false
tags: ["mariadb", "galera"]
categories: ["mariadb"]
subtitle: ""
descriptions: ""
bigimg:
---

mysql系列之集群 MariaDB Galera Cluster 部署

## 参考
[How To Setup MariaDB Galera Cluster 10.0 On CentOS](http://www.unixmen.com/setup-mariadb-galera-cluster-10-0-centos/)
http://www.unixmen.com/setup-mariadb-galera-cluster-10-0-centos/

## 安装 过程


*MariaDB* is a relational database management system (RDBMS) and  [MariaDB Galera Cluster](https://mariadb.com/kb/en/mariadb/what-is-mariadb-galera-cluster/) is a synchronous multi-master cluster for MariaDB. It is available on Linux only, and only supports the *XtraDB/InnoDB* storage engines. This article explains how to setup MariaDB Galera Cluster 10.0 with 3 nodes running on CentOS 6.5(tom注：在CentOS 7.0上也成功) x86_64 resulting in a HA (high-availability) database cluster.

#### CLUSTER DETAILS

We using 3 freshly deployed VMs running a minimal install of CentOS 6.5 x86_64.

    Cluster node 1 has hostname db1 and IP address 1.1.1.1
    Cluster node 2 has hostname db2 and IP address 1.1.1.2
    Cluster node 3 has hostname db3 and IP address 1.1.1.3

## Step 1: Add MariaDB Repositories

Create a mariadb repository `/etc/yum.repos.d/mariadb.repo` using following content in your system.

For CentOS 7 – 64bit:

    [mariadb]
    name = MariaDB
    baseurl = http://yum.mariadb.org/10.0/centos7-amd64
    gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
    gpgcheck=1

For CentOS 7 – 32bit:

    [mariadb]
    name = MariaDB
    baseurl = http://yum.mariadb.org/10.0/centos7-x86
    gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
    gpgcheck=1

## Step 2 – Set SELinux in permissive mode

Before starting the setup put SELinux into permissive mode on all nodes:

    sudo setenforce 0

## Step 3 – Install MariaDB Galera Cluster 10.0 software

If you did a CentOS 6 minimal installation then make sure you install the socat package from the EPEL repository before proceeding with installing the MariaDB Galera Cluster 10.0 software.

You can install socat package directly from EPEL with the following command (for x86_64):

    sudo yum install http://dl.fedoraproject.org/pub/epel/6/x86_64/socat-1.7.2.3-1.el6.x86_64.rpm

On CentOS 7 you can install socat package with following command.

    sudo yum install socat -y

Install the MariaDB Galera Cluster 10.0 software by executing the following command on all nodes:

    sudo yum install MariaDB-Galera-server MariaDB-client rsync galera -y

如果说，这一步没有安装成功，请参考之前的那一篇[mysql系列之集群 MariaDB-Galera-server 安装](tomtsang.github.io/2016/11/15/mysql-1/)

## Step 4:  Setup MariaDB security

Start the mysql ( init script in MariaDB 10.0 is still called mysql)

    sudo service mysql start
Run the `mysql_secure_installation` script so we can improve the security. Run the following command on all nodes:

    sudo /usr/bin/mysql_secure_installation
I choose password as ‘`dbpass`’ and accepted all defaults (so answered `yes` to all questions).

## Step 5 – Create MariaDB Galera Cluster users

Now, we have to create some users that must be able to access the database. The ‘sst_user’ is the user which a database node will use for authenticating to another database node in the State Transfer Snapshot (SST) phase. Run the following command on all nodes:

    mysql -u root -p
    mysql> DELETE FROM mysql.user WHERE user='';
    mysql> GRANT ALL ON *.* TO 'root'@'%' IDENTIFIED BY 'dbpass';
    mysql> GRANT USAGE ON *.* to sst_user@'%' IDENTIFIED BY 'dbpass';
    mysql> GRANT ALL PRIVILEGES on *.* to sst_user@'%';
    mysql> FLUSH PRIVILEGES;
    mysql> quit
You are suggested to change ‘`%`’ to hostname(s) or IP addresses from which those users can access the database. Because ‘`%`’ means that the `root` or `sst_user` is allowed to access the database from any host, So less security.

## Step 6 – Create the MariaDB Galera Cluster config

First stop the mysql services on all nodes:

    sudo service mysql stop
Next, We are going to create the MariaDB Galera Cluster configuration by the following command on all nodes (go through the IMPORTANT NOTE after the config and make required changes for db2, and db3):

    sudo cat >> /etc/my.cnf.d/server.cnf << EOF

    [mariadb-10.0]
    binlog_format=ROW
    default-storage-engine=innodb
    innodb_autoinc_lock_mode=2
    innodb_locks_unsafe_for_binlog=1
    query_cache_size=0
    query_cache_type=0
    bind-address=0.0.0.0
    datadir=/var/lib/mysql
    innodb_log_file_size=100M
    innodb_file_per_table
    innodb_flush_log_at_trx_commit=2
    wsrep_provider=/usr/lib64/galera/libgalera_smm.so
    wsrep_cluster_address="gcomm://1.1.1.1,1.1.1.2,1.1.1.3"
    wsrep_cluster_name='galera_cluster'
    wsrep_node_address='1.1.1.1'
    wsrep_node_name='db1'
    wsrep_sst_method=rsync
    wsrep_sst_auth=sst_user:dbpass
    EOF
*IMPORTANT NOTE*: when executing this command on db2 and db3 do not forget to adjust the `wsrep_node_address` and `wsrep_node_name` variables.

On db2 :

    wsrep_node_address=1.1.1.2
    wsrep_node_name='db2'
On db3 :

    wsrep_node_address='1.1.1.3'
    wsrep_node_name='db3'

## Step 7– Initialize the first cluster node

Start MariaDB with the special ‘`‐‐wsrep-new-cluster`’ option , Do it on node `db1` only so the primary node of the cluster is initialized:

    sudo /etc/init.d/mysql start --wsrep-new-cluster
Check status by run the following command on node db1 only:

    mysql -u root -p -e "show status like 'wsrep%';"
Some important information in the output are the following lines:

    wsrep_local_state_comment | Synced <-- cluster is synced
    wsrep_incoming_addresses  | 1.1.1.1:3306 <-- node db1 is a provider
    wsrep_cluster_size        | 1 <-- cluster consists of 1 node
    wsrep_ready               | ON <-- good :)

如果说这里报错了，请参考[mysql系列之集群 MariaDB-Galera-cluster 报错](tomtsang.github.io/)

## Step 8– Add the other cluster nodes

Check and confirm nodes db2 and db3 have the correct configuration in `/etc/my.cnf.d/server.cnf` under the [`mariadb-10.0`] as described in step 6.

With the correct configuration in place, all that is required to make db2 and db3 a member of the cluster is to start them like you would start any regular service. On db2 issue the following command:

    sudo service mysql start
Check what has changed in the cluster status by executing the following command on db1 or db2:

    mysql -u root -p -e "show status like 'wsrep%';"
And you will see that node db2 is now known as the cluster size is ‘2’ and the IP address of node db2 is listed:

    | wsrep_local_state_comment | Synced                    |
    | wsrep_incoming_addre sses | 1.1.1.1:3306,1.1.1.2:3306 |
    | wsrep_cluster_size        | 2                         |
    | wsrep_connected           | ON                        |
    | wsrep_ready               | ON                        |
Repeat the same step for node db3. On node db3 only execute the following command

    sudo service mysql start
Check what has changed in the cluster status by executing the following command on for example db1:

    mysql -u root -p -e "show status like 'wsrep%';"
And you should see that node db3 is now known as the cluster size is ‘3’ and the IP address of node db3 is listed:

    | wsrep_local_state_comment | Synced                                 |
    | wsrep_incoming_addresses  | 1.1.1.3:3306,1.1.1.1:3306,1.1.1.2:3306 |
    | wsrep_cluster_size        | 3                                      |
    | wsrep_connected           | ON                                     |
    | wsrep_ready               | ON                                     |

## Step 9 – Verify replication

Now the cluster is running. Let’s test whether it is working. On db1 create a database ‘clustertest’ by run the following command:

    mysql -u root -p -e 'CREATE DATABASE clustertest;'

    mysql -u root -p -e 'CREATE TABLE clustertest.mycluster ( id INT NOT NULL AUTO_INCREMENT, name VARCHAR(50), ipaddress VARCHAR(20), PRIMARY KEY(id));'

    mysql -u root -p -e 'INSERT INTO clustertest.mycluster (name, ipaddress) VALUES ("db1", "1.1.1.1");'

Check if the database, table and data exists:

    mysql -u root -p -e 'SELECT * FROM clustertest.mycluster;'
    Enter password:
    +----+------+-----------+
    | id | name | ipaddress |
    +----+------+-----------+
    | 2  | db1  | 1.1.1.1   |
    +----+------+-----------+
Now do the check on node db2:

    mysql -u root -p -e 'SELECT * FROM clustertest.mycluster;'
    Enter password:
    +----+------+-----------+
    | id | name | ipaddress |
    +----+------+-----------+
    | 2  | db1  | 1.1.1.1   |
    +----+------+-----------+
Now do the same check on node db3:

    mysql -u root -p -e 'SELECT * FROM clustertest.mycluster;'
    Enter password:
    +----+------+-----------+
    | id | name | ipaddress |
    +----+------+-----------+
    | 2  | db1  | 1.1.1.1   |
    +----+------+-----------+
From these outputs we can confirm that everything was successfully replicated by node db1 across all other nodes.

That’s it.

Cheers!


## ctrl + v 吧

    sudo vi /etc/hosts
        10.10.162.11    db1
        10.10.162.12    db2
        10.10.162.13    db3
    sudo vi /etc/yum.repos.d/mariadb.repo
        # MariaDB 10.1 CentOS repository list - created 2016-11-15 02:38 UTC
        # http://downloads.mariadb.org/mariadb/repositories/
        [mariadb]
        name = MariaDB
        baseurl = http://yum.mariadb.org/10.0/centos7-amd64
        gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
        gpgcheck=1
    sudo setenforce 0
    sudo yum install socat -y
    sudo yum install socat MariaDB-Galera-server MariaDB-client rsync galera -y
    sudo service mysql start
    sudo /usr/bin/mysql_secure_installation
    mysql -u root -p
    sudo service mysql stop
    sudo cat >> /etc/my.cnf.d/server.cnf << EOF
        binlog_format=ROW
        default-storage-engine=innodb
        innodb_autoinc_lock_mode=2
        innodb_locks_unsafe_for_binlog=1
        query_cache_size=0
        query_cache_type=0
        bind-address=0.0.0.0
        datadir=/var/lib/mysql
        innodb_log_file_size=100M
        innodb_file_per_table
        innodb_flush_log_at_trx_commit=2
        wsrep_provider=/usr/lib64/galera/libgalera_smm.so
        wsrep_cluster_address="gcomm://1.1.1.1,1.1.1.2,1.1.1.3"
        wsrep_cluster_name='galera_cluster'
        wsrep_node_address='1.1.1.1'
        wsrep_node_name='db1'
        wsrep_sst_method=rsync
        wsrep_sst_auth=sst_user:dbpass
        EOF
    # 上面这个好像会出错，算了，我们 sudo vi 吧
    sudo vi /etc/my.cnf.d/server.cnf
    sudo cp -a /etc/my.cnf.d/server.cnf .
    ls
    sudo cp -a server.cnf server.cnf.12
    sudo cp -a server.cnf server.cnf.13
    sudo vi server.cnf.12
    sudo vi server.cnf.13
    scp server.cnf.12 spa@s12:/home/spa/  # 之后在spa@s12中改一改吧。
    scp server.cnf.13 spa@s13:/home/spa/
    # 好了，现在三台机器的配置都完成啦！
    sudo /etc/init.d/mysql start --wsrep-new-cluster
    mysql -u root -p -e "show status like 'wsrep%';"
    # 好了，如果没有问题，我们把其它2台机器也启动下。 sudo service mysql start; sudo service mysql status;
    mysql -u root -p -e 'CREATE DATABASE clustertest;'
    mysql -u root -p -e 'CREATE TABLE clustertest.mycluster ( id INT NOT NULL AUTO_INCREMENT, name VARCHAR(50), ipaddress VARCHAR(20), PRIMARY KEY(id));'
    mysql -u root -p -e 'INSERT INTO clustertest.mycluster (name, ipaddress) VALUES ("db1", "1.1.1.1");'
    mysql -u root -p -e 'SELECT * FROM clustertest.mycluster;'
    # 到其它2台机器上也查一下吧。 mysql -u root -p -e 'SELECT * FROM clustertest.mycluster;'
    # 如果其它机器也显示出来了，那就Cheers!
