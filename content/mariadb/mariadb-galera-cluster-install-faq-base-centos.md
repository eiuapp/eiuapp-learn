---
title: "Mariadb Galera Cluster Install Faq Base Centos"
date: 2018-03-02T12:44:14+08:00
draft: false
tags: ["mariadb", "galera"]
categories: ["mariadb"]
subtitle: ""
descriptions: ""
bigimg:
---

mysql系列之集群 MariaDB-Galera-cluster 报错 


## 报错1, /etc/my.cnf.d/下 有一个 不应该存在的 .cnf

错误原因：

mysql 加载了 /etc/my.cnf.d/下的所有 `*.cnf` 文件，其中又有一个之前的不应该存在的 `*.cnf`。

正确情况下，应该只有4个`.cnf`文件。
    [spa@s11 ~]$ ls /etc/my.cnf.d/
    client.cnf  mysql-clients.cnf  server.cnf  tokudb.cnf
    [spa@s11 ~]$

错误情况：

    [tom@kube-node-13 ~]$ sudo /etc/init.d/mysql start --wsrep-new-cluster
    Starting MySQL.161116 17:44:54 mysqld_safe Logging to '/data/mariadb/mysql/kube-node-13.err'.
    . SUCCESS!
    [tom@kube-node-13 ~]$ mysql -u root -p -e "show status like 'wsrep%'"
    Enter password:
    +--------------------------+----------------------+
    | Variable_name            | Value                |
    +--------------------------+----------------------+
    | wsrep_cluster_conf_id    | 18446744073709551615 |
    | wsrep_cluster_size       | 0                    |
    | wsrep_cluster_state_uuid |                      |
    | wsrep_cluster_status     | Disconnected         |
    | wsrep_connected          | OFF                  |
    | wsrep_local_bf_aborts    | 0                    |
    | wsrep_local_index        | 18446744073709551615 |
    | wsrep_provider_name      |                      |
    | wsrep_provider_vendor    |                      |
    | wsrep_provider_version   |                      |
    | wsrep_ready              | ON                   |
    | wsrep_thread_count       | 0                    |
    +--------------------------+----------------------+
    [tom@kube-node-13 ~]$

解决过程：

怎么办呢？

先看状态

    [tom@kube-node-13 ~]$ sudo service mysql status -l
     SUCCESS! MySQL running (11946)
关闭掉，我们来手动启动查原因。

    [tom@kube-node-13 ~]$ sudo service mysql stop
    Shutting down MySQL... SUCCESS!
    [tom@kube-node-13 ~]$

查一下，如何手动启动

    [spa@s11 ~]$ which mysqld
    /usr/sbin/mysqld
    [cdn@test_240 OpenPicker]$ mysqld --help
    mysqld  Ver 10.0.28-MariaDB-wsrep for Linux on x86_64 (MariaDB Server, wsrep_25.16.rc3fc46e)
    Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

    Starts the MariaDB database server.

    Usage: mysqld [OPTIONS]

    For more help options (several pages), use mysqld --verbose --help.
    [cdn@test_240 OpenPicker]$ use mysqld --verbose --help
    ...
    一大串
    ...

手动启动

    [tom@kube-node-13 ~]$ mysqld --verbose
    161116 18:30:01 [Note] mysqld (mysqld 10.0.28-MariaDB-wsrep) starting as process 12713 ...
    161116 18:30:01 [Warning] Can't create test file /data/mariadb/mysql/kube-node-13.lower-test
    161116 18:30:01 [Note] WSREP: Read nil XID from storage engines, skipping position init
    161116 18:30:01 [Note] WSREP: wsrep_load(): loading provider library '/usr/lib/galera/libgalera_smm.so'
    161116 18:30:01 [ERROR] WSREP: wsrep_load(): dlopen(): /usr/lib/galera/libgalera_smm.so: cannot open shared object file: No such file or directory
    161116 18:30:01 [ERROR] WSREP: wsrep_load(/usr/lib/galera/libgalera_smm.so) failed: Invalid argument (22). Reverting to no provider.
    161116 18:30:01 [Note] WSREP: Read nil XID from storage engines, skipping position init
    161116 18:30:01 [Note] WSREP: wsrep_load(): loading provider library 'none'
    2016-11-16 18:30:01 7f1069f42880 InnoDB: Warning: Using innodb_locks_unsafe_for_binlog is DEPRECATED. This option may be removed in future releases. Please use READ COMMITTED transaction isolation level instead, see http://dev.mysql.com/doc/refman/5.6/en/set-transaction.html.
    161116 18:30:01 [Note] InnoDB: Using mutexes to ref count buffer pool pages
    161116 18:30:01 [Note] InnoDB: The InnoDB memory heap is disabled
    161116 18:30:01 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
    161116 18:30:01 [Note] InnoDB: GCC builtin __atomic_thread_fence() is used for memory barrier
    161116 18:30:01 [Note] InnoDB: Compressed tables use zlib 1.2.7
    161116 18:30:01 [Note] InnoDB: Using Linux native AIO
    161116 18:30:01 [Note] InnoDB: Using CPU crc32 instructions
    161116 18:30:01 [Note] InnoDB: Initializing buffer pool, size = 128.0M
    161116 18:30:01 [Note] InnoDB: Completed initialization of buffer pool
    161116 18:30:01 [ERROR] InnoDB: ./ibdata1 can't be opened in read-write mode
    161116 18:30:01 [ERROR] InnoDB: The system tablespace must be writable!
    161116 18:30:01 [ERROR] Plugin 'InnoDB' init function returned error.
    161116 18:30:01 [ERROR] Plugin 'InnoDB' registration as a STORAGE ENGINE failed.
    161116 18:30:01 [ERROR] mysqld: File '/data/mariadb/mysql/aria_log_control' not found (Errcode: 13 "Permission denied")
    161116 18:30:01 [ERROR] mysqld: Got error 'Can't open file' when trying to use aria control file '/data/mariadb/mysql/aria_log_control'
    161116 18:30:01 [ERROR] Plugin 'Aria' init function returned error.
    161116 18:30:01 [ERROR] Plugin 'Aria' registration as a STORAGE ENGINE failed.
    161116 18:30:01 [Note] Plugin 'FEEDBACK' is disabled.
    161116 18:30:01 [ERROR] Can't open the mysql.plugin table. Please run mysql_upgrade to create it.
    161116 18:30:01 [ERROR] Unknown/unsupported storage engine: innodb
    161116 18:30:01 [ERROR] Aborting

    161116 18:30:01 [Note] WSREP: Service disconnected.
    161116 18:30:02 [Note] WSREP: Some threads may fail to exit.
    161116 18:30:02 [Note] mysqld: Shutdown complete

看到了吧，上面有一行

`161116 18:30:01 [ERROR] WSREP: wsrep_load(): dlopen(): /usr/lib/galera/libgalera_smm.so: cannot open shared object file: No such file or directory`

说明有问题了吧。
先这样，我们确保一下，启动程序的用户是mysql,来吧。

    [tom@kube-node-13 ~]$ sudo --help
    sudo: invalid option -- '-'
    usage: sudo [-D level] -h | -K | -k | -V
    usage: sudo -v [-AknS] [-D level] [-g groupname|#gid] [-p prompt] [-u user name|#uid]
    usage: sudo -l[l] [-AknS] [-D level] [-g groupname|#gid] [-p prompt] [-U user name] [-u user name|#uid] [-g groupname|#gid] [command]
    usage: sudo [-AbEHknPS] [-r role] [-t type] [-C fd] [-D level] [-g groupname|#gid] [-p prompt] [-u user name|#uid] [-g groupname|#gid] [VAR=value] [-i|-s] [<command>]
    usage: sudo -e [-AknS] [-r role] [-t type] [-C fd] [-D level] [-g groupname|#gid] [-p prompt] [-u user name|#uid] file ...
    [tom@kube-node-13 ~]$
好了，知道了，用sudo -u ,那这个 -u 后面接什么用户呢？mysql ,还是 mysqld？

    [tom@kube-node-13 ~]$ ll /var/lib/mysql/
    total 217128
    -rw-rw----. 1 mysql mysql     16384 11月 16 18:18 aria_log.00000001
    -rw-rw----. 1 mysql mysql        52 11月 16 18:18 aria_log_control
    -rw-rw----. 1 mysql mysql  12582912 11月 16 18:18 ibdata1
    -rw-rw----. 1 mysql mysql 104857600 11月 16 18:18 ib_logfile0
    -rw-rw----. 1 mysql mysql 104857600 11月 16 18:18 ib_logfile1
    -rw-r-----. 1 mysql root       5489 11月 16 18:18 kube-node-13.err
    -rw-rw----. 1 mysql mysql         0 11月 16 13:34 multi-master.info
    drwx--x--x. 2 mysql mysql      4096 11月 16 12:51 mysql
    srwxrwxrwx. 1 mysql mysql         0 11月 17 10:28 mysql.sock
    drwx------. 2 mysql mysql      4096 11月 16 12:51 performance_schema
    -rw-------. 1 mysql root       2538 11月 16 18:18 wsrep_recovery.N15shv
    [tom@kube-node-13 ~]$

好了，现在知道了，这些文件都是 `mysql`用户的，那自然，我们应该是 `sudo -u mysql` .
来吧。

    [tom@kube-node-13 ~]$ sudo -u mysql mysqld --verbose
    161116 18:32:38 [Note] mysqld (mysqld 10.0.28-MariaDB-wsrep) starting as process 12963 ...
    161116 18:32:38 [Note] WSREP: Read nil XID from storage engines, skipping position init
    161116 18:32:38 [Note] WSREP: wsrep_load(): loading provider library '/usr/lib/galera/libgalera_smm.so'
    161116 18:32:38 [ERROR] WSREP: wsrep_load(): dlopen(): /usr/lib/galera/libgalera_smm.so: cannot open shared object file: No such file or directory
    161116 18:32:38 [ERROR] WSREP: wsrep_load(/usr/lib/galera/libgalera_smm.so) failed: Invalid argument (22). Reverting to no provider.
    161116 18:32:38 [Note] WSREP: Read nil XID from storage engines, skipping position init
    161116 18:32:38 [Note] WSREP: wsrep_load(): loading provider library 'none'
    2016-11-16 18:32:38 7f71c04dd880 InnoDB: Warning: Using innodb_locks_unsafe_for_binlog is DEPRECATED. This option may be removed in future releases. Please use READ COMMITTED transaction isolation level instead, see http://dev.mysql.com/doc/refman/5.6/en/set-transaction.html.
    161116 18:32:38 [Note] InnoDB: Using mutexes to ref count buffer pool pages
    161116 18:32:38 [Note] InnoDB: The InnoDB memory heap is disabled
    161116 18:32:38 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
    161116 18:32:38 [Note] InnoDB: GCC builtin __atomic_thread_fence() is used for memory barrier
    161116 18:32:38 [Note] InnoDB: Compressed tables use zlib 1.2.7
    161116 18:32:38 [Note] InnoDB: Using Linux native AIO
    161116 18:32:38 [Note] InnoDB: Using CPU crc32 instructions
    161116 18:32:38 [Note] InnoDB: Initializing buffer pool, size = 128.0M
    161116 18:32:38 [Note] InnoDB: Completed initialization of buffer pool
    161116 18:32:38 [Note] InnoDB: Highest supported file format is Barracuda.
    161116 18:32:38 [Note] InnoDB: 128 rollback segment(s) are active.
    161116 18:32:38 [Note] InnoDB: Waiting for purge to start
    161116 18:32:38 [Note] InnoDB:  Percona XtraDB (http://www.percona.com) 5.6.32-79.0 started; log sequence number 1624176
    161116 18:32:38 [Note] Plugin 'FEEDBACK' is disabled.
    161116 18:32:38 [Note] Server socket created on IP: '0.0.0.0'.
    161116 18:32:38 [Note] WSREP: Read nil XID from storage engines, skipping position init
    161116 18:32:38 [Note] WSREP: wsrep_load(): loading provider library 'none'
    161116 18:32:38 [Note] mysqld: ready for connections.
    Version: '10.0.28-MariaDB-wsrep'  socket: '/var/lib/mysql/mysql.sock'  port: 3306  MariaDB Server, wsrep_25.16.rc3fc46e
    ^C^C^C^C^C^CKilled
    [tom@kube-node-13 ~]$

看到了。错误

`161116 18:32:38 [ERROR] WSREP: wsrep_load(): dlopen(): /usr/lib/galera/libgalera_smm.so: cannot open shared object file: No such file or directory`

还在。

找一下，看这个文件在不在了？

    [tom@kube-node-13 ~]$ sudo ls -l /usr/lib/galera/libgalera_smm.so
    ls: cannot access /usr/lib/galera/libgalera_smm.so: No such file or directory
好了，这个文件真的不存在。
那试一下 /usr/lib64/ 下有没有这个文件呢？

    [tom@kube-node-13 ~]$ sudo ls  /usr/lib64/galera/libgalera_smm.so
    /usr/lib64/galera/libgalera_smm.so
哈哈。有呀。
那看一下，我们配置里面的是哪个？

    [tom@kube-node-13 ~]$ sudo cat /etc/my.cnf.d/server.cnf
    ...
    binlog_format=ROW
    default-storage-engine=innodb
    innodb_autoinc_lock_mode=2
    innodb_locks_unsafe_for_binlog=1
    query_cache_size=0
    query_cache_type=0
    bind-address=0.0.0.0
    datadir=/data/mariadb/mysql
    #datadir=/var/lib/mysql
    innodb_log_file_size=100M
    innodb_file_per_table
    innodb_flush_log_at_trx_commit=2
    wsrep_provider=/usr/lib64/galera/libgalera_smm.so
    wsrep_cluster_address="gcomm://10.10.13.110,192.168.31.240,10.10.12.13"
    wsrep_cluster_name='galera_cluster'
    wsrep_node_address='10.10.12.13'
    wsrep_node_name='db3'
    wsrep_sst_method=rsync
    wsrep_sst_auth=sst_user:dbpass

看到了吧，是`wsrep_provider=/usr/lib64/galera/libgalera_smm.so`。是`/usr/lib64/`。那说明了，程序没有成功加载我们的配置文件呀。

无论怎样，我们先做一个软链接，看下，能不能成功。

注意哈，这里是直接把整个文件夹做成软链接，不要写错了。不是`sudo ln -s /usr/lib64/galera/ /usr/lib/galera/`， 更不是`sudo ln -s /usr/lib64/galera/ /usr/lib/galera/`。

    [tom@kube-node-13 galera]$ sudo ln -s /usr/lib64/galera /usr/lib/galera
    [tom@kube-node-13 galera]$ cd /usr/lib/galera/
    [tom@kube-node-13 galera]$ ls
    libgalera_smm.so
    [tom@kube-node-13 galera]$

做好了。再来一次。

    [tom@kube-node-13 galera]$ sudo -u mysql mysqld --verbose
    161116 18:46:07 [Note] mysqld (mysqld 10.0.28-MariaDB-wsrep) starting as process 14216 ...
    161116 18:46:07 [Note] WSREP: Read nil XID from storage engines, skipping position init
    161116 18:46:07 [Note] WSREP: wsrep_load(): loading provider library '/usr/lib/galera/libgalera_smm.so'
    161116 18:46:07 [Note] WSREP: wsrep_load(): Galera 25.3.18(r3632) by Codership Oy <info@codership.com> loaded successfully.
    161116 18:46:07 [Note] WSREP: CRC-32C: using hardware acceleration.
    161116 18:46:07 [Warning] WSREP: Could not open state file for reading: '/data/mariadb/mysql//grastate.dat'
    161116 18:46:07 [Note] WSREP: Found saved state: 00000000-0000-0000-0000-000000000000:-1
    161116 18:46:07 [Note] WSREP: Passing config to GCS: base_dir = /data/mariadb/mysql/; base_host = 10.10.12.13; base_port = 4567; cert.log_conflicts = no; debug = no; evs.auto_evict = 0; evs.delay_margin = PT1S; evs.delayed_keep_period = PT30S; evs.inactive_check_period = PT0.5S; evs.inactive_timeout = PT15S; evs.join_retrans_period = PT1S; evs.max_install_timeouts = 3; evs.send_window = 4; evs.stats_report_period = PT1M; evs.suspect_timeout = PT5S; evs.user_send_window = 2; evs.view_forget_timeout = PT24H; gcache.dir = /data/mariadb/mysql/; gcache.keep_pages_size = 0; gcache.mem_size = 0; gcache.name = /data/mariadb/mysql//galera.cache; gcache.page_size = 128M; gcache.size = 128M; gcomm.thread_prio = ; gcs.fc_debug = 0; gcs.fc_factor = 1.0; gcs.fc_limit = 16; gcs.fc_master_slave = no; gcs.max_packet_size = 64500; gcs.max_throttle = 0.25; gcs.recv_q_hard_limit = 9223372036854775807; gcs.recv_q_soft_limit = 0.25; gcs.sync_donor = no; gmcast.segment = 0; gmcast.version = 0; pc.announce_timeout = PT3S; pc.checksum = false; pc.ignore_q
    161116 18:46:07 [Note] WSREP: Service thread queue flushed.
    161116 18:46:07 [Note] WSREP: Assign initial position for certification: -1, protocol version: -1
    161116 18:46:07 [Note] WSREP: wsrep_sst_grab()
    161116 18:46:07 [Note] WSREP: Start replication
    161116 18:46:07 [Note] WSREP: Setting initial position to 00000000-0000-0000-0000-000000000000:-1
    161116 18:46:07 [Note] WSREP: protonet asio version 0
    161116 18:46:07 [Note] WSREP: Using CRC-32C for message checksums.
    161116 18:46:07 [Note] WSREP: backend: asio
    161116 18:46:07 [Note] WSREP: gcomm thread scheduling priority set to other:0
    161116 18:46:07 [Warning] WSREP: access file(/data/mariadb/mysql//gvwstate.dat) failed(No such file or directory)
    161116 18:46:07 [Note] WSREP: restore pc from disk failed
    161116 18:46:07 [Note] WSREP: GMCast version 0
    161116 18:46:07 [Note] WSREP: (e0f5cb2a, 'tcp://0.0.0.0:4567') listening at tcp://0.0.0.0:4567
    161116 18:46:07 [Note] WSREP: (e0f5cb2a, 'tcp://0.0.0.0:4567') multicast: , ttl: 1
    161116 18:46:07 [Note] WSREP: EVS version 0
    161116 18:46:07 [Note] WSREP: gcomm: connecting to group 'my_wsrep_cluster', peer '10.10.13.110:'
    161116 18:46:10 [Warning] WSREP: no nodes coming from prim view, prim not possible
    161116 18:46:10 [Note] WSREP: view(view_id(NON_PRIM,e0f5cb2a,1) memb {
    	e0f5cb2a,0
    } joined {
    } left {
    } partitioned {
    })
    161116 18:46:10 [Warning] WSREP: last inactive check more than PT1.5S ago (PT3.50279S), skipping check
    161116 18:46:40 [Note] WSREP: view((empty))
    161116 18:46:40 [ERROR] WSREP: failed to open gcomm backend connection: 110: failed to reach primary view: 110 (Connection timed out)
    	 at gcomm/src/pc.cpp:connect():162
    161116 18:46:40 [ERROR] WSREP: gcs/src/gcs_core.cpp:gcs_core_open():208: Failed to open backend connection: -110 (Connection timed out)
    161116 18:46:40 [ERROR] WSREP: gcs/src/gcs.cpp:gcs_open():1380: Failed to open channel 'my_wsrep_cluster' at 'gcomm://10.10.13.110': -110 (Connection timed out)
    161116 18:46:40 [ERROR] WSREP: gcs connect failed: Connection timed out
    161116 18:46:40 [ERROR] WSREP: wsrep::connect(gcomm://10.10.13.110) failed: 7
    161116 18:46:40 [ERROR] Aborting

    161116 18:46:40 [Note] WSREP: Service disconnected.
    161116 18:46:41 [Note] WSREP: Some threads may fail to exit.
    161116 18:46:41 [Note] mysqld: Shutdown complete

    [tom@kube-node-13 galera]$

哈哈，看 `161116 18:46:07 [Note] WSREP: wsrep_load(): loading provider library '/usr/lib/galera/libgalera_smm.so'` 知道，这个错误，确实是跳过去了。
现在来找第一个 [Warning] 。找到了。`161116 18:46:07 [Warning] WSREP: Could not open state file for reading: '/data/mariadb/mysql//grastate.dat'`
那看下，这个文件有没有？

    [tom@kube-node-13 galera]$ cd /data/mariadb/mysql
    [tom@kube-node-13 mysql]$ ls -lZ
    -rw-rw----. mysql mysql unconfined_u:object_r:mysqld_db_t:s0 aria_log.00000001
    -rw-rw----. mysql mysql unconfined_u:object_r:mysqld_db_t:s0 aria_log_control
    -rw-------. mysql mysql unconfined_u:object_r:mysqld_db_t:s0 galera.cache
    -rw-rw----. mysql mysql unconfined_u:object_r:mysqld_db_t:s0 grastate.dat
    -rw-rw----. mysql mysql unconfined_u:object_r:mysqld_db_t:s0 ibdata1
    -rw-rw----. mysql mysql unconfined_u:object_r:mysqld_db_t:s0 ib_logfile0
    -rw-rw----. mysql mysql unconfined_u:object_r:mysqld_db_t:s0 ib_logfile1
    -rw-r-----. mysql root  unconfined_u:object_r:mysqld_db_t:s0 kube-node-13.err
    -rw-rw----. mysql mysql unconfined_u:object_r:mysqld_db_t:s0 kube-node-13.pid
    -rw-rw----. mysql mysql unconfined_u:object_r:mysqld_db_t:s0 multi-master.info
    drwx--x--x. mysql mysql unconfined_u:object_r:mysqld_db_t:s0 mysql
    drwx------. mysql mysql unconfined_u:object_r:mysqld_db_t:s0 performance_schema
    -rw-rw-rw-. root  root  unconfined_u:object_r:mysqld_db_t:s0 slow_query_log.log

有`/data/mariadb/mysql//grastate.dat`呀。什么鬼？
不要手动启动了。用sudo service 启动一下。

    [tom@kube-node-13 mysql]$ sudo service mysql start
    Starting MySQL SUCCESS!
    [tom@kube-node-13 mysql]$ 161116 18:50:43 mysqld_safe Logging to '/data/mariadb/mysql/kube-node-13.err'.
    ^C
    [tom@kube-node-13 mysql]$

好了，再把现有的 mysqld 关闭了去。

    [tom@kube-node-13 mysql]$ sudo service mysql stop
    Shutting down MySQL161116 18:54:00 [Note] mysqld: Normal shutdown

    .161116 18:54:00 [Note] WSREP: Stop replication
    161116 18:54:00 [Note] Event Scheduler: Purging the queue. 0 events
    161116 18:54:00 [Note] InnoDB: FTS optimize thread exiting.
    161116 18:54:00 [Note] InnoDB: Starting shutdown...
    161116 18:54:01 [Note] InnoDB: Waiting for page_cleaner to finish flushing of buffer pool
    ..161116 18:54:02 [Note] InnoDB: Shutdown completed; log sequence number 1624196
    161116 18:54:02 [Note] mysqld: Shutdown complete

     SUCCESS!
    [tom@kube-node-13 mysql]$ ps -ef | grep mysql
    tom      15073 36010  0 18:54 pts/1    00:00:00 grep --color=auto mysql

    [tom@kube-node-13 mysql]$ sudo service mysql start
    Starting MySQL.161116 18:54:29 mysqld_safe Logging to '/data/mariadb/mysql/kube-node-13.err'.
    ..
    ................................ ERROR!
    [tom@kube-node-13 mysql]$

有错误日志了。看去。

    [tom@kube-node-13 mysql]$ sudo cat /data/mariadb/mysql/kube-node-13.err
    ...
    ...
    ...
    161116 18:19:18 [Note] WSREP: wsrep_load(): loading provider library 'none'
    161116 18:19:18 [Note] /usr/sbin/mysqld: ready for connections.
    Version: '10.0.28-MariaDB-wsrep'  socket: '/var/lib/mysql/mysql.sock'  port: 3306  MariaDB Server, wsrep_25.16.rc3fc46e
    161116 18:29:53 [Note] /usr/sbin/mysqld: Normal shutdown

    161116 18:29:53 [Note] WSREP: Stop replication
    161116 18:29:53 [Note] Event Scheduler: Purging the queue. 0 events
    161116 18:29:53 [Note] InnoDB: FTS optimize thread exiting.
    161116 18:29:53 [Note] InnoDB: Starting shutdown...
    161116 18:29:54 [Note] InnoDB: Waiting for page_cleaner to finish flushing of buffer pool
    161116 18:29:56 [Note] InnoDB: Shutdown completed; log sequence number 1624176
    161116 18:29:56 [Note] /usr/sbin/mysqld: Shutdown complete

    161116 18:29:56 mysqld_safe mysqld from pid file /data/mariadb/mysql/kube-node-13.pid ended
    161116 18:54:29 mysqld_safe Starting mysqld daemon with databases from /data/mariadb/mysql
    161116 18:54:29 mysqld_safe WSREP: Running position recovery with --log_error='/data/mariadb/mysql/wsrep_recovery.N6pzuB' --pid-file='/data/mariadb/mysql/kube-node-13-recover.pid'
    161116 18:54:29 [Note] /usr/sbin/mysqld (mysqld 10.0.28-MariaDB-wsrep) starting as process 15313 ...
    161116 18:54:32 mysqld_safe WSREP: Recovered position 00000000-0000-0000-0000-000000000000:-1
    161116 18:54:32 [Note] /usr/sbin/mysqld (mysqld 10.0.28-MariaDB-wsrep) starting as process 15379 ...
    161116 18:54:32 [Note] WSREP: Read nil XID from storage engines, skipping position init
    161116 18:54:32 [Note] WSREP: wsrep_load(): loading provider library '/usr/lib/galera/libgalera_smm.so'
    161116 18:54:32 [Note] WSREP: wsrep_load(): Galera 25.3.18(r3632) by Codership Oy <info@codership.com> loaded successfully.
    161116 18:54:32 [Note] WSREP: CRC-32C: using hardware acceleration.
    161116 18:54:32 [Note] WSREP: Found saved state: 00000000-0000-0000-0000-000000000000:-1
    161116 18:54:32 [Note] WSREP: Passing config to GCS: base_dir = /data/mariadb/mysql/; base_host = 10.10.12.13; base_port = 4567; cert.log_conflicts = no; debug = no; evs.auto_evict = 0; evs.delay_margin = PT1S; evs.delayed_keep_period = PT30S; evs.inactive_check_period = PT0.5S; evs.inactive_timeout = PT15S; evs.join_retrans_period = PT1S; evs.max_install_timeouts = 3; evs.send_window = 4; evs.stats_report_period = PT1M; evs.suspect_timeout = PT5S; evs.user_send_window = 2; evs.view_forget_timeout = PT24H; gcache.dir = /data/mariadb/mysql/; gcache.keep_pages_size = 0; gcache.mem_size = 0; gcache.name = /data/mariadb/mysql//galera.cache; gcache.page_size = 128M; gcache.size = 128M; gcomm.thread_prio = ; gcs.fc_debug = 0; gcs.fc_factor = 1.0; gcs.fc_limit = 16; gcs.fc_master_slave = no; gcs.max_packet_size = 64500; gcs.max_throttle = 0.25; gcs.recv_q_hard_limit = 9223372036854775807; gcs.recv_q_soft_limit = 0.25; gcs.sync_donor = no; gmcast.segment = 0; gmcast.version = 0; pc.announce_timeout = PT3S; pc.checksum = false; pc.ignore_q
    161116 18:54:32 [Note] WSREP: Service thread queue flushed.
    161116 18:54:32 [Note] WSREP: Assign initial position for certification: -1, protocol version: -1
    161116 18:54:32 [Note] WSREP: wsrep_sst_grab()
    161116 18:54:32 [Note] WSREP: Start replication
    161116 18:54:32 [Note] WSREP: Setting initial position to 00000000-0000-0000-0000-000000000000:-1
    161116 18:54:32 [Note] WSREP: protonet asio version 0
    161116 18:54:32 [Note] WSREP: Using CRC-32C for message checksums.
    161116 18:54:32 [Note] WSREP: backend: asio
    161116 18:54:32 [Note] WSREP: gcomm thread scheduling priority set to other:0
    161116 18:54:32 [Warning] WSREP: access file(/data/mariadb/mysql//gvwstate.dat) failed(No such file or directory)
    161116 18:54:32 [Note] WSREP: restore pc from disk failed
    161116 18:54:32 [Note] WSREP: GMCast version 0
    161116 18:54:32 [Note] WSREP: (0e6fea53, 'tcp://0.0.0.0:4567') listening at tcp://0.0.0.0:4567
    161116 18:54:32 [Note] WSREP: (0e6fea53, 'tcp://0.0.0.0:4567') multicast: , ttl: 1
    161116 18:54:32 [Note] WSREP: EVS version 0
    161116 18:54:32 [Note] WSREP: gcomm: connecting to group 'my_wsrep_cluster', peer '10.10.13.110:'
    161116 18:54:35 [Warning] WSREP: no nodes coming from prim view, prim not possible
    161116 18:54:35 [Note] WSREP: view(view_id(NON_PRIM,0e6fea53,1) memb {
    	0e6fea53,0
    } joined {
    } left {
    } partitioned {
    })
    161116 18:54:36 [Warning] WSREP: last inactive check more than PT1.5S ago (PT3.50217S), skipping check
    161116 18:55:05 [Note] WSREP: view((empty))
    161116 18:55:05 [ERROR] WSREP: failed to open gcomm backend connection: 110: failed to reach primary view: 110 (Connection timed out)
    	 at gcomm/src/pc.cpp:connect():162
    161116 18:55:05 [ERROR] WSREP: gcs/src/gcs_core.cpp:gcs_core_open():208: Failed to open backend connection: -110 (Connection timed out)
    161116 18:55:05 [ERROR] WSREP: gcs/src/gcs.cpp:gcs_open():1380: Failed to open channel 'my_wsrep_cluster' at 'gcomm://10.10.13.110': -110 (Connection timed out)
    161116 18:55:05 [ERROR] WSREP: gcs connect failed: Connection timed out
    161116 18:55:05 [ERROR] WSREP: wsrep::connect(gcomm://10.10.13.110) failed: 7
    161116 18:55:05 [ERROR] Aborting

    161116 18:55:05 [Note] WSREP: Service disconnected.
    161116 18:55:06 [Note] WSREP: Some threads may fail to exit.
    161116 18:55:06 [Note] /usr/sbin/mysqld: Shutdown complete

    161116 18:55:06 mysqld_safe mysqld from pid file /data/mariadb/mysql/kube-node-13.pid ended
    [tom@kube-node-13 mysql]$ date
    2016年 11月 16日 星期三 18:55:16 CST
    [tom@kube-node-13 mysql]$

嗯，确实是现在的日志。
来看一下。

    161116 18:54:32 [Warning] WSREP: access file(/data/mariadb/mysql//gvwstate.dat) failed(No such file or directory)
    161116 18:54:32 [Note] WSREP: restore pc from disk failed
    161116 18:54:32 [Note] WSREP: GMCast version 0
    161116 18:54:32 [Note] WSREP: (0e6fea53, 'tcp://0.0.0.0:4567') listening at tcp://0.0.0.0:4567
    161116 18:54:32 [Note] WSREP: (0e6fea53, 'tcp://0.0.0.0:4567') multicast: , ttl: 1
    161116 18:54:32 [Note] WSREP: EVS version 0
    161116 18:54:32 [Note] WSREP: gcomm: connecting to group 'my_wsrep_cluster', peer '10.10.13.110:'

哈。有问题呀。我们在配置文件中的 `wsrep_cluster_address="gcomm://10.10.13.110,192.168.31.240,10.10.12.13"
wsrep_cluster_name='galera_cluster'` 到这里怎么是这样的啦！！！

好吧。快回忆，是哪里配置过 `my_wsrep_cluster`.

想到了。
之前在 `/etc/my.cnf.d/wsrep.cnf` 里面配置过这个。

    [tom@kube-node-13 mysql]$ cd /etc/my.cnf.d/
    [tom@kube-node-13 my.cnf.d]$ ls -la
    total 40
    drwxr-xr-x.   2 root root 4096 11月 16 19:09 .
    drwxr-xr-x. 141 root root 8192 11月 16 16:45 ..
    -rw-r--r--.   1 root root  295 10月 28 01:16 client.cnf
    -rw-r--r--.   1 root root  232 10月 28 01:16 mysql-clients.cnf
    -rw-r--r--.   1 root root 1772 11月 16 18:59 server.cnf
    -rw-r--r--.   1 root root 1007 11月 16 14:10 server.cnf_original
    -rw-r--r--.   1 root root  285 11月  3 09:14 tokudb.cnf
    -rw-r--r--.   1 root root 3611 11月 16 15:10 wsrep.cnf
    [tom@kube-node-13 my.cnf.d]$ vi wsrep.cnf

查了一下，里面确实就是有呀。哭了。原来是这个文件的
那把这个文件移出去，再试一下吧。

    [tom@kube-node-13 my.cnf.d]$ sudo mv wsrep.cnf ~/wsrep.cnf.bak
    [tom@kube-node-13 my.cnf.d]$ ls
    client.cnf  mysql-clients.cnf  server.cnf  server.cnf_original  tokudb.cnf
    [tom@kube-node-13 my.cnf.d]$ sudo service mysql status
     ERROR! MySQL is not running, but lock file (/var/lock/subsys/mysql) exists
    [tom@kube-node-13 my.cnf.d]$ sudo service mysql stop
     ERROR! MySQL server PID file could not be found!
    [tom@kube-node-13 my.cnf.d]$ sudo service mysql start
    Starting MySQL.161116 19:12:20 mysqld_safe Logging to '/data/mariadb/mysql/kube-node-13.err'.
    ............a...................... ERROR!
    [tom@kube-node-13 my.cnf.d]$

崩溃，看日志。

    [tom@kube-node-13 my.cnf.d]$ sudo cat /data/mariadb/mysql/kube-node-13.err
    ...
    ...
    ...
    161116 19:02:19 mysqld_safe mysqld from pid file /data/mariadb/mysql/kube-node-13.pid ended
    161116 19:12:20 mysqld_safe Starting mysqld daemon with databases from /data/mariadb/mysql
    161116 19:12:20 mysqld_safe WSREP: Running position recovery with --log_error='/data/mariadb/mysql/wsrep_recovery.3kyk74' --pid-file='/data/mariadb/mysql/kube-node-13-recover.pid'
    161116 19:12:20 [Note] /usr/sbin/mysqld (mysqld 10.0.28-MariaDB-wsrep) starting as process 17474 ...
    161116 19:12:23 mysqld_safe WSREP: Recovered position 00000000-0000-0000-0000-000000000000:-1
    161116 19:12:23 [Note] /usr/sbin/mysqld (mysqld 10.0.28-MariaDB-wsrep) starting as process 17514 ...
    161116 19:12:23 [Note] WSREP: Read nil XID from storage engines, skipping position init
    161116 19:12:23 [Note] WSREP: wsrep_load(): loading provider library '/usr/lib64/galera/libgalera_smm.so'
    161116 19:12:23 [Note] WSREP: wsrep_load(): Galera 25.3.18(r3632) by Codership Oy <info@codership.com> loaded successfully.
    161116 19:12:23 [Note] WSREP: CRC-32C: using hardware acceleration.
    161116 19:12:23 [Note] WSREP: Found saved state: 00000000-0000-0000-0000-000000000000:-1
    161116 19:12:23 [Note] WSREP: Passing config to GCS: base_dir = /data/mariadb/mysql/; base_host = 10.10.12.13; base_port = 4567; cert.log_conflicts = no; debug = no; evs.auto_evict = 0; evs.delay_margin = PT1S; evs.delayed_keep_period = PT30S; evs.inactive_check_period = PT0.5S; evs.inactive_timeout = PT15S; evs.join_retrans_period = PT1S; evs.max_install_timeouts = 3; evs.send_window = 4; evs.stats_report_period = PT1M; evs.suspect_timeout = PT5S; evs.user_send_window = 2; evs.view_forget_timeout = PT24H; gcache.dir = /data/mariadb/mysql/; gcache.keep_pages_size = 0; gcache.mem_size = 0; gcache.name = /data/mariadb/mysql//galera.cache; gcache.page_size = 128M; gcache.size = 128M; gcomm.thread_prio = ; gcs.fc_debug = 0; gcs.fc_factor = 1.0; gcs.fc_limit = 16; gcs.fc_master_slave = no; gcs.max_packet_size = 64500; gcs.max_throttle = 0.25; gcs.recv_q_hard_limit = 9223372036854775807; gcs.recv_q_soft_limit = 0.25; gcs.sync_donor = no; gmcast.segment = 0; gmcast.version = 0; pc.announce_timeout = PT3S; pc.checksum = false; pc.ignore_q
    161116 19:12:23 [Note] WSREP: Service thread queue flushed.
    161116 19:12:23 [Note] WSREP: Assign initial position for certification: -1, protocol version: -1
    161116 19:12:23 [Note] WSREP: wsrep_sst_grab()
    161116 19:12:23 [Note] WSREP: Start replication
    161116 19:12:23 [Note] WSREP: Setting initial position to 00000000-0000-0000-0000-000000000000:-1
    161116 19:12:23 [Note] WSREP: protonet asio version 0
    161116 19:12:23 [Note] WSREP: Using CRC-32C for message checksums.
    161116 19:12:23 [Note] WSREP: backend: asio
    161116 19:12:23 [Note] WSREP: gcomm thread scheduling priority set to other:0
    161116 19:12:23 [Warning] WSREP: access file(/data/mariadb/mysql//gvwstate.dat) failed(No such file or directory)
    161116 19:12:23 [Note] WSREP: restore pc from disk failed
    161116 19:12:23 [Note] WSREP: GMCast version 0
    161116 19:12:23 [Note] WSREP: (8c84d2c7, 'tcp://0.0.0.0:4567') listening at tcp://0.0.0.0:4567
    161116 19:12:23 [Note] WSREP: (8c84d2c7, 'tcp://0.0.0.0:4567') multicast: , ttl: 1
    161116 19:12:23 [Note] WSREP: EVS version 0
    161116 19:12:23 [Note] WSREP: gcomm: connecting to group 'galera_cluster', peer '10.10.12.13:,10.10.13.110:,192.168.31.240:'
    161116 19:12:23 [Note] WSREP: (8c84d2c7, 'tcp://0.0.0.0:4567') connection established to 8c84d2c7 tcp://10.10.12.13:4567
    161116 19:12:23 [Warning] WSREP: (8c84d2c7, 'tcp://0.0.0.0:4567') address 'tcp://10.10.12.13:4567' points to own listening address, blacklisting
    161116 19:12:23 [Note] WSREP: (8c84d2c7, 'tcp://0.0.0.0:4567') connection established to 8c84d2c7 tcp://10.10.12.13:4567
    161116 19:12:26 [Warning] WSREP: no nodes coming from prim view, prim not possible
    161116 19:12:26 [Note] WSREP: view(view_id(NON_PRIM,8c84d2c7,1) memb {
    	8c84d2c7,0
    } joined {
    } left {
    } partitioned {
    })
    161116 19:12:26 [Warning] WSREP: last inactive check more than PT1.5S ago (PT3.50305S), skipping check
    161116 19:12:56 [Note] WSREP: view((empty))
    161116 19:12:56 [ERROR] WSREP: failed to open gcomm backend connection: 110: failed to reach primary view: 110 (Connection timed out)
    	 at gcomm/src/pc.cpp:connect():162
    161116 19:12:56 [ERROR] WSREP: gcs/src/gcs_core.cpp:gcs_core_open():208: Failed to open backend connection: -110 (Connection timed out)
    161116 19:12:56 [ERROR] WSREP: gcs/src/gcs.cpp:gcs_open():1380: Failed to open channel 'galera_cluster' at 'gcomm://10.10.12.13,10.10.13.110,192.168.31.240': -110 (Connection timed out)
    161116 19:12:56 [ERROR] WSREP: gcs connect failed: Connection timed out
    161116 19:12:56 [ERROR] WSREP: wsrep::connect(gcomm://10.10.12.13,10.10.13.110,192.168.31.240) failed: 7
    161116 19:12:56 [ERROR] Aborting

    161116 19:12:56 [Note] WSREP: Service disconnected.
    161116 19:12:57 [Note] WSREP: Some threads may fail to exit.
    161116 19:12:57 [Note] /usr/sbin/mysqld: Shutdown complete

    161116 19:12:57 mysqld_safe mysqld from pid file /data/mariadb/mysql/kube-node-13.pid ended
    [tom@kube-node-13 my.cnf.d]$

好事情。
`161116 19:12:23 [Note] WSREP: wsrep_load(): loading provider library '/usr/lib64/galera/libgalera_smm.so'`
`161116 19:12:23 [Note] WSREP: gcomm: connecting to group 'galera_cluster', peer '10.10.12.13:,10.10.13.110:,192.168.31.240:'`
`161116 19:12:23 [Note] WSREP: (8c84d2c7, 'tcp://0.0.0.0:4567') connection established to 8c84d2c7 tcp://10.10.12.13:4567`
这几个关键的点，都正常了。

但是为什么还出错了呢。崩溃了吧。

清醒一下，让我想想，为什么？

叮咚。

明白了。刚刚是，只启动cluster的第一台机器，命令错了哈。应该用 `sudo /etc/init.d/mysql start --wsrep-new-cluster`，下回要注意呀。

    [tom@kube-node-13 my.cnf.d]$ sudo /etc/init.d/mysql start --wsrep-new-cluster
    Starting MySQL.161116 19:17:28 mysqld_safe Logging to '/data/mariadb/mysql/kube-node-13.err'.
    . SUCCESS!
    [tom@kube-node-13 my.cnf.d]$ mysql -u root -p -e "show status like 'wsrep%'";
    Enter password:
    +------------------------------+---------------------------------------------+
    | Variable_name                | Value                                       |
    +------------------------------+---------------------------------------------+
    | wsrep_local_state_uuid       | 440fe21c-abee-11e6-b1c1-9bfbe1e4335d        |
    | wsrep_protocol_version       | 7                                           |
    | wsrep_last_committed         | 0                                           |
    | wsrep_replicated             | 0                                           |
    | wsrep_replicated_bytes       | 0                                           |
    | wsrep_repl_keys              | 0                                           |
    | wsrep_repl_keys_bytes        | 0                                           |
    | wsrep_repl_data_bytes        | 0                                           |
    | wsrep_repl_other_bytes       | 0                                           |
    | wsrep_received               | 2                                           |
    | wsrep_received_bytes         | 138                                         |
    | wsrep_local_commits          | 0                                           |
    | wsrep_local_cert_failures    | 0                                           |
    | wsrep_local_replays          | 0                                           |
    | wsrep_local_send_queue       | 0                                           |
    | wsrep_local_send_queue_max   | 1                                           |
    | wsrep_local_send_queue_min   | 0                                           |
    | wsrep_local_send_queue_avg   | 0.000000                                    |
    | wsrep_local_recv_queue       | 0                                           |
    | wsrep_local_recv_queue_max   | 2                                           |
    | wsrep_local_recv_queue_min   | 0                                           |
    | wsrep_local_recv_queue_avg   | 0.500000                                    |
    | wsrep_local_cached_downto    | 18446744073709551615                        |
    | wsrep_flow_control_paused_ns | 0                                           |
    | wsrep_flow_control_paused    | 0.000000                                    |
    | wsrep_flow_control_sent      | 0                                           |
    | wsrep_flow_control_recv      | 0                                           |
    | wsrep_cert_deps_distance     | 0.000000                                    |
    | wsrep_apply_oooe             | 0.000000                                    |
    | wsrep_apply_oool             | 0.000000                                    |
    | wsrep_apply_window           | 0.000000                                    |
    | wsrep_commit_oooe            | 0.000000                                    |
    | wsrep_commit_oool            | 0.000000                                    |
    | wsrep_commit_window          | 0.000000                                    |
    | wsrep_local_state            | 4                                           |
    | wsrep_local_state_comment    | Synced                                      |
    | wsrep_cert_index_size        | 0                                           |
    | wsrep_causal_reads           | 0                                           |
    | wsrep_cert_interval          | 0.000000                                    |
    | wsrep_incoming_addresses     | 10.10.12.13:3306                            |
    | wsrep_desync_count           | 0                                           |
    | wsrep_evs_delayed            |                                             |
    | wsrep_evs_evict_list         |                                             |
    | wsrep_evs_repl_latency       | 4.903e-06/9.301e-06/1.892e-05/5.26662e-06/5 |
    | wsrep_evs_state              | OPERATIONAL                                 |
    | wsrep_gcomm_uuid             | 440ea66b-abee-11e6-9185-f7b2a306dbf2        |
    | wsrep_cluster_conf_id        | 1                                           |
    | wsrep_cluster_size           | 1                                           |
    | wsrep_cluster_state_uuid     | 440fe21c-abee-11e6-b1c1-9bfbe1e4335d        |
    | wsrep_cluster_status         | Primary                                     |
    | wsrep_connected              | ON                                          |
    | wsrep_local_bf_aborts        | 0                                           |
    | wsrep_local_index            | 0                                           |
    | wsrep_provider_name          | Galera                                      |
    | wsrep_provider_vendor        | Codership Oy <info@codership.com>           |
    | wsrep_provider_version       | 25.3.18(r3632)                              |
    | wsrep_ready                  | ON                                          |
    | wsrep_thread_count           | 2                                           |
    +------------------------------+---------------------------------------------+
    [tom@kube-node-13 my.cnf.d]$

终于是成功了!
终于是成功了!
终于是成功了!
