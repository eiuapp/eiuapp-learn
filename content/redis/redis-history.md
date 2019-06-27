---
title: "Redis History"
date: 2018-02-03T13:41:32+08:00
draft: false
tags: ["redis"]
categories: ["redis"]
subtitle: "redis draft"
descriptions: ""
bigimg: ""
---

之前 31.240 的 redis 的history

```
[tom@kube-node-15 redis-3.0.7]$ src/redis-cli 
Could not connect to Redis at 127.0.0.1:6379: Connection refused
not connected> exit
[tom@kube-node-15 redis-3.0.7]$ sudo make install 
cd src && make install
make[1]: Entering directory `/opt/redis-3.0.7/src'

Hint: It's a good idea to run 'make test' ;)

    INSTALL install
    INSTALL install
    INSTALL install
    INSTALL install
    INSTALL install
make[1]: Leaving directory `/opt/redis-3.0.7/src'
[tom@kube-node-15 redis-3.0.7]$ pwd
/opt/redis-3.0.7
[tom@kube-node-15 redis-3.0.7]$ 

[tom@kube-node-15 redis-3.0.7]$ redis-cli
Could not connect to Redis at 127.0.0.1:6379: Connection refused
not connected> 
[tom@kube-node-15 redis-3.0.7]$ which redis-cli
/usr/local/bin/redis-cli
[tom@kube-node-15 redis-3.0.7]$ cd /usr/local/bin
[tom@kube-node-15 bin]$  ls
redis-benchmark  redis-check-aof  redis-check-dump  redis-cli  redis-sentinel  redis-server
[tom@kube-node-15 bin]$ cd /opt/redis-3.0.7/[tom@kube-node-15 redis-3.0.7]$ cd utils/
[tom@kube-node-15 utils]$ ls
build-static-symbols.tcl  create-cluster            hyperloglog        lru           redis-copy.rb      redis_init_script.tpl  speed-regression.tcl
cluster_fail_time.tcl     generate-command-help.rb  install_server.sh  mkrelease.sh  redis_init_script  redis-sha1.rb          whatisdoing.sh
[tom@kube-node-15 utils]$ 
[tom@kube-node-15 utils]$ sudo  ./install_server.sh 
Welcome to the redis service installer
This script will help you easily set up a running redis server

Please select the redis port for this instance: [6379] 
Selecting default: 6379
Please select the redis config file name [/etc/redis/6379.conf] 
Selected default - /etc/redis/6379.conf
Please select the redis log file name [/var/log/redis_6379.log] 
Selected default - /var/log/redis_6379.log
Please select the data directory for this instance [/var/lib/redis/6379] 
Selected default - /var/lib/redis/6379
Please select the redis executable path [] /usr/local/bin/redis-server
Selected config:
Port           : 6379
Config file    : /etc/redis/6379.conf
Log file       : /var/log/redis_6379.log
Data dir       : /var/lib/redis/6379
Executable     : /usr/local/bin/redis-server
Cli Executable : /usr/local/bin/redis-cli
Is this ok? Then press ENTER to go on or Ctrl-C to abort.
Copied /tmp/6379.conf => /etc/init.d/redis_6379
Installing service...
Successfully added to chkconfig!
Successfully added to runlevels 345!
Starting Redis server...
Installation successful!
[tom@kube-node-15 utils]$ 
[tom@kube-node-15 utils]$ sudo chkconfig --add redis_6379
[tom@kube-node-15 utils]$ sudo chkconfig --level  345
chkconfig version 1.3.61 - Copyright (C) 1997-2000 Red Hat, Inc.
This may be freely redistributed under the terms of the GNU Public License.

usage:   chkconfig [--list] [--type <type>] [name]
         chkconfig --add <name>
         chkconfig --del <name>
         chkconfig --override <name>
         chkconfig [--level <levels>] [--type <type>] <name> <on|off|reset|resetpriorities>
[tom@kube-node-15 utils]$ sudo chkconfig --level  345 redis_6379 on
[tom@kube-node-15 utils]$ sudo reboot
```



