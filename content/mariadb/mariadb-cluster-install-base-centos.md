---
title: "Mariadb Cluster Install Base Centos"
date: 2018-03-02T12:45:19+08:00
draft: false
tags: ["mysql", "mariadb", "cluster", "install"]
categories: ["mysql"]
subtitle: ""
descriptions: ""
bigimg:
---

mysql系列之mysql-cluster

## 主要参考

http://mariadb.org/

## 环境

1. centos7-amd64
2. mariadb10.1（IP:10.10.13.110） + mariadb5.5(IP:192.168.31.240)


## 步骤

### 准备2台机器

安装mariadb10.1

[参考这里](http://localhost:4000/2016/11/15/mysql-install/)

### 把 mariadb5.5 的数据，导出后，导入到 mariadb10.1

##### 导出

##### 导入


mysql主从复制
	第一步，就要看这几个参考
依次看
		http://blog.csdn.net/gaowenhui2008/article/details/46698321
		http://blog.csdn.net/hguisu/article/details/7325124
		http://blog.jobbole.com/94595/
		http://www.xuejiehome.com/blread-1664.html
	MYSQL主从同步的管理
		参考
			http://blog.csdn.net/gaowenhui2008/article/details/46698321
			1.  停止MYSQL同步
		1.  停止MYSQL同步
			STOP SLAVE IO_THREAD;    #停止IO进程
			STOP SLAVE SQL_THREAD;    #停止SQL进程
			STOP SLAVE;                               #停止IO和SQL进程
		2.  启动MYSQL同步
			START SLAVE IO_THREAD;    #启动IO进程
			START SLAVE SQL_THREAD;  #启动SQL进程
			START SLAVE;                             #启动IO和SQL进程
		3.   重置MYSQL同步
			RESET SLAVE;
			用于让从属服务器忘记其在主服务器的二进制日志中的复制位置, 它会删除master.info和relay-log.info文件，以及所有的中继日志，并启动一个新的中继日志,当你不需要主从的时候可以在从上执行这个操作。不然以后还会同步，可能会覆盖掉你的数据库，我以前就遇到过这样傻叉的事情。哈哈！
		4.   查看MYSQL同步状态
			SHOW SLAVE STATUS;
			这个命令主要查看Slave_IO_Running、Slave_SQL_Running、Seconds_Behind_Master、Last_IO_Error、Last_SQL_Error这些值来把握复制的状态。
		5.  临时跳过MYSQL同步错误
			经常会朋友mysql主从同步遇到错误的时候，比如一个主键冲突等，那么我就需要在确保那一行数据一致的情况下临时的跳过这个错误，那就需要使用SQL_SLAVE_SKIP_COUNTER = n命令了，n是表示跳过后面的n个事件,比如我跳过一个事件的操作如下：
			STOP SLAVE;
			SET GLOBAL SQL_SLAVE_SKIP_COUNTER=1;
			START SLAVE;
		6.  从指定位置重新同步
			有的时候主从同步有问题了以后，需要从log位置的下一个位置进行同步，相当于跳过那个错误，这时候也可以使用CHANGE MASTER命令来处理，只要找到对应的LOG位置就可以,比如：
			CHANGE MASTER TO MASTER_HOST='10.1.1.75',MASTER_USER='replication',MASTER_PASSWORD='123456',MASTER_LOG_FILE='mysql-bin.000006', MASTER_LOG_POS=106;
			START SLAVE;
		MYSQL主从同步的管理经验介绍
			1.   不要乱使用SQL_SLAVE_SKIP_COUNTER命令。
				这个命令跳过之后很可能会导致你的主从数据不一致，一定要先将指定的错误记录下来，然后再去检查数据是否一致，尤其是核心的业务数据。
			2.   结合percona-toolkit工具pt-table-checksum定期查看数据是否一致
				这个是DBA必须要定期做的事情，呵呵，有合适的工具何乐而不为呢？另外percona-toolkit还提供了对数据库不一致的解决方案，可以采用pt-table-sync，这个工具不会更改主的数据。还可以使用pt-heartbeat来查看从服务器的复制落后情况。
				具体的请查看：http://blog.chinaunix.net/uid-20639775-id-3229211.html。
			3.   使用replicate-wild-ignore-table选项而不要使用replicate-do-db或者replicate-ignore-db。
				原因已经在上面做了说明。
			4.   将主服务器的日志模式调整成mixed。
			5.   每个表都加上主键，主键对数据库的同步会有影响尤其是居于ROW复制模式。
	　mysql主从复制
情况１：主从mysql，之前都无数据
		参考
			http://blog.jobbole.com/94595/
			http://www.xuejiehome.com/blread-1664.html
		步骤
			目标
				10.10.12.14(主)
10.10.12.15(从)
			1、主从安装mysql，版本一致
				mysql>status;
					　MariaDB [test]> status;
--------------
mysql  Ver 15.1 Distrib 5.5.44-MariaDB, for Linux (x86_64) using readline 5.1

Connection id:		4
Current database:	test
Current user:		root@localhost
SSL:			Not in use
Current pager:		stdout
Using outfile:		''
Using delimiter:	;
Server:			MariaDB
Server version:		5.5.44-MariaDB-log MariaDB Server
Protocol version:	10
Connection:		Localhost via UNIX socket
Server characterset:	utf8
Db     characterset:	utf8
Client characterset:	utf8
Conn.  characterset:	utf8
UNIX socket:		/var/lib/mysql/mysql.sock
Uptime:			18 min 15 sec

Threads: 2  Questions: 24  Slow queries: 0  Opens: 1  Flush tables: 2  Open tables: 27  Queries per second avg: 0.021
--------------

MariaDB [test]>

			2、修改master，slave服务器
				master服务器配置：
					sudo　　vi /etc/my.cnf
						　[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd
log-bin=mysql-bin
server-id=14
binlog-ignore-db = mysql,information_schema


[mysqld]
server-id=14     
#设置服务器唯一的id，默认是1，一般取IP最后一段，我们设置ip最后一段，slave设置14
log-bin=mysql-bin
# 启用二进制日志
binlog-ignore-db = mysql,information_schema  
#忽略写入binlog的库
				slave服务器配置：
					sudo　　vi /etc/my.cnf
						　[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd
log-bin=mysql-bin
server-id=15
replicate-wild-ignore-table=test.%
#replicate-do-db = test
#slave-skip-errors = all

							replicate-wild-ignore-table=test.%
#　replicate-do-db = test     
#上面２句都可以表示只同步test库(但是第一句更好，不要使用第２名)，　
＃如果同步所有的库，则不要加上这一句
slave-skip-errors = all   
#忽略因复制出现的所有错误
			3、重启主从服务器mysql
				sudo systemctl restart mariadb.service
				sudo systemctl status mariadb.service
			4、在主服务器上建立帐户并授权slave
				GRANT REPLICATION SLAVE ON *.* to 'sync'@'10.10.12.%' identified by '123.com';
					注意，这里是　
10.10.12.%, 而不是
10.10.12.*
我犯过这样的错误
				GRANT REPLICATION SLAVE ON *.* to 'sync'@'10.10.12.15' identified by '123.com';
				注意：大家在设置权限的时候不要将密码设置过于简单！
			5、查看主数据库状态
				　show master status\G;
					　MariaDB [(none)]> show master status;
+------------------+----------+--------------+--------------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB         |
+------------------+----------+--------------+--------------------------+
| mysql-bin.000002 |      707 |              | mysql,information_schema |
+------------------+----------+--------------+--------------------------+
1 row in set (0.00 sec)

MariaDB [(none)]>
					　MariaDB [(none)]> show master status\G;
*************************** 1. row ***************************
            File: mysql-bin.000002
        Position: 707
    Binlog_Do_DB:
Binlog_Ignore_DB: mysql,information_schema
1 row in set (0.01 sec)

ERROR: No query specified

MariaDB [(none)]>
				记录下 File 及 Position 的值，在后面进行从服务器操作的时候需要用到。
			6、配置从数据库
				change master to
master_host='10.10.12.14',
master_user='sync',
master_password='123.com',
master_log_file'='mysql-bin.000002',
master_log_pos=707;
			　7、启动slave同步进程并
				MariaDB [(none)]> start slave;
			查看状态
				　　MariaDB [(none)]> show slave status\G;
					　MariaDB [(none)]> show slave status\G;
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 10.10.12.14
                  Master_User: sync
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000002
          Read_Master_Log_Pos: 707
               Relay_Log_File: mariadb-relay-bin.000002
                Relay_Log_Pos: 838
        Relay_Master_Log_File: mysql-bin.000002
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: test
				其中Slave_IO_Running 与 Slave_SQL_Running 的值都必须为YES，才表明状态正常。
				报错
					主从同步出现一下错误：Slave_IO_Running: Connecting
						导致lave_IO_Running 为connecting 的原因主要有以下 3 个方面：
						1、网络不通
						2、密码不对
							解决
								从服务器
									STOP SLAVE;                               #停止IO和SQL进程
									RESET SLAVE;
									STOP SLAVE;                               #停止IO和SQL进程
									6、配置从数据库
										change master to
master_host='10.10.12.14',
master_user='sync',
master_password='123.com',
master_log_file'='mysql-bin.000002',
master_log_pos=707;
									START SLAVE;                             #启动IO和SQL进程
									实操
										　MariaDB [(none)]> reset slave;
ERROR 1198 (HY000): This operation cannot be performed with a running slave; run STOP SLAVE first
MariaDB [(none)]> stop slave;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> reset slave;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> show slave status\G;
MariaDB [(none)]> stop slave;
Query OK, 0 rows affected, 1 warning (0.00 sec)

MariaDB [(none)]> change master to
    -> master_host='10.10.13.100',
    -> master_user='sync',
    -> master_password='sync.com',
    -> master_log_file='mysql-bin.000001',
    -> master_log_pos=927;
Query OK, 0 rows affected (0.01 sec)

MariaDB [(none)]> start slave;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> show slave status\G;
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 10.10.13.100
                  Master_User: sync
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000001
          Read_Master_Log_Pos: 927
               Relay_Log_File: mariadb-relay-bin.000002
                Relay_Log_Pos: 529
        Relay_Master_Log_File: mysql-bin.000001
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
						3、pos不对
			8、验证主从同步
