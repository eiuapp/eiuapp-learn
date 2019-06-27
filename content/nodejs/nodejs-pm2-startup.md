---
title: "Nodejs Version"
date: 2019-06-09T15:08:46+08:00
draft: true
tags: ["nodejs", "pm2"]
categories: ["nodejs"]
subtitle: ""
descriptions: ""
bigimg:
---


nodejs系列之pm2开机自启动

## pm2设置开机自启动命令

http://pm2.keymetrics.io/docs/usage/startup/#generating-a-startup-script
https://www.cnblogs.com/duhuo/p/5587247.html

- pm2 startup，这个命令会在系统 `/etc/systemd/system/` 路径下生成一个 `pm2-root.service` 文件用来开机启动 pm2 服务。
- pm2 save, 保存当前 pm2 运行的各个应用保存到 `/root/.pm2/dump.pm2` 下，开机重启时读取该文件中的内容启动相关应用。

```bash
ubuntu@utuntu:~/lcnx/local/lvchuang-admin-server$ pm2 save
[PM2] Saving current process list...
[PM2] Successfully saved in /home/ubuntu/.pm2/dump.pm2
ubuntu@utuntu:~/lcnx/local/lvchuang-admin-server$ 
ubuntu@utuntu:~/lcnx/local/lvchuang-admin-server$ pm2 startup
[PM2] Init System found: systemd
[PM2] To setup the Startup Script, copy/paste the following command:
sudo env PATH=$PATH:/home/ubuntu/.nvm/versions/node/v10.15.3/bin /home/ubuntu/.nvm/versions/node/v11.15.0/lib/node_modules/pm2/bin/pm2 startup systemd -u ubuntu --hp /home/ubuntu
ubuntu@utuntu:~/lcnx/local/lvchuang-admin-server$ sudo pm2 startup
[sudo] password for ubuntu: 
sudo: pm2: command not found
ubuntu@utuntu:~/lcnx/local/lvchuang-admin-server$ sudo env PATH=$PATH:/home/ubuntu/.nvm/versions/node/v10.15.3/bin /home/ubuntu/.nvm/versions/node/v11.15.0/lib/node_modules/pm2/bin/pm2 startup systemd -u ubuntu --hp /home/ubuntu
[PM2] Init System found: systemd
Platform systemd
Template
[Unit]
Description=PM2 process manager
Documentation=https://pm2.keymetrics.io/
After=network.target

[Service]
Type=forking
User=ubuntu
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
Environment=PATH=/home/ubuntu/.local/bin:/home/ubuntu/.nvm/versions/node/v10.15.3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/ubuntu/lcnx/aliyun/env/java/bin:/home/ubuntu/.nvm/versions/node/v11.15.0/bin:/home/ubuntu/.nvm/versions/node/v10.15.3/bin:/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
Environment=PM2_HOME=/home/ubuntu/.pm2
PIDFile=/home/ubuntu/.pm2/pm2.pid
Restart=on-failure

ExecStart=/home/ubuntu/.nvm/versions/node/v11.15.0/lib/node_modules/pm2/bin/pm2 resurrect
ExecReload=/home/ubuntu/.nvm/versions/node/v11.15.0/lib/node_modules/pm2/bin/pm2 reload all
ExecStop=/home/ubuntu/.nvm/versions/node/v11.15.0/lib/node_modules/pm2/bin/pm2 kill

[Install]
WantedBy=multi-user.target

Target path
/etc/systemd/system/pm2-ubuntu.service
Command list
[ 'systemctl enable pm2-ubuntu' ]
[PM2] Writing init configuration in /etc/systemd/system/pm2-ubuntu.service
[PM2] Making script booting at startup...
[PM2] [-] Executing: systemctl enable pm2-ubuntu...
Created symlink /etc/systemd/system/multi-user.target.wants/pm2-ubuntu.service → /etc/systemd/system/pm2-ubuntu.service.
[PM2] [v] Command successfully executed.
+---------------------------------------+
[PM2] Freeze a process list on reboot via:
$ pm2 save

[PM2] Remove init script via:
$ pm2 unstartup systemd
ubuntu@utuntu:~/lcnx/local/lvchuang-admin-server$ ls /etc/systemd/system/pm2-ubuntu.service 
/etc/systemd/system/pm2-ubuntu.service
ubuntu@utuntu:~/lcnx/local/lvchuang-admin-server$ cat /etc/systemd/system/pm2-ubuntu.service 
[Unit]
Description=PM2 process manager
Documentation=https://pm2.keymetrics.io/
After=network.target

[Service]
Type=forking
User=ubuntu
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
Environment=PATH=/home/ubuntu/.local/bin:/home/ubuntu/.nvm/versions/node/v10.15.3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/ubuntu/lcnx/aliyun/env/java/bin:/home/ubuntu/.nvm/versions/node/v11.15.0/bin:/home/ubuntu/.nvm/versions/node/v10.15.3/bin:/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
Environment=PM2_HOME=/home/ubuntu/.pm2
PIDFile=/home/ubuntu/.pm2/pm2.pid
Restart=on-failure

ExecStart=/home/ubuntu/.nvm/versions/node/v11.15.0/lib/node_modules/pm2/bin/pm2 resurrect
ExecReload=/home/ubuntu/.nvm/versions/node/v11.15.0/lib/node_modules/pm2/bin/pm2 reload all
ExecStop=/home/ubuntu/.nvm/versions/node/v11.15.0/lib/node_modules/pm2/bin/pm2 kill

[Install]
WantedBy=multi-user.target
ubuntu@utuntu:~/lcnx/local/lvchuang-admin-server$ 
```


## systemctl status pm2-ubuntu.service 不对

PM2 not start after system reboot

```
Can't open PID file /home/ubuntu/.pm2/pm2.pid (yet?) after start
```

https://github.com/Unitech/pm2/issues/2775#issuecomment-373574946

