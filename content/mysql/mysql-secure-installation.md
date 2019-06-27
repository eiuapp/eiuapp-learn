---
title: "Mysql Secure Installation"
date: 2018-03-02T12:41:05+08:00
draft: false
tags: ["mysql"]
categories: ["mysql"]
subtitle: ""
descriptions: ""
bigimg:
---

mysql系列之mysql_secure_installation

## 参考
http://www.myexception.cn/mysql/1902013.html

## 过程

安装完mysql-server 会提示可以运行mysql_secure_installation。
运行mysql_secure_installation会执行几个设置：
  a)为root用户设置密码
  b)删除匿名账号
  c)取消root用户远程登录
  d)删除test库和对test库的访问权限
  e)刷新授权表使修改生效
通过这几项的设置能够提高mysql库的安全。建议生产环境中mysql安装这完成后一定要运行一次mysql_secure_installation，详细步骤请参看下面的命令:

    [root@dns ~]# mysql_secure_installation
    /usr/bin/mysql_secure_installation:行379: find_mysql_client: 未找到命令

    NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
          SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

    In order to log into MariaDB to secure it, we'll need the current
    password for the root user.  If you've just installed MariaDB, and
    you haven't set the root password yet, the password will be blank,
    so you should just press enter here.

    ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
    Enter current password for root (enter for none):   -->初次运行直接enter
    OK, successfully used password, moving on...

    Setting the root password ensures that nobody can log into the MariaDB
    root user without the proper authorisation.

    Set root password? [Y/n] Y
    New password:
    Re-enter new password:
    Password updated successfully!
    Reloading privilege tables..
     ... Success!


    By default, a MariaDB installation has an anonymous user, allowing anyone
    to log into MariaDB without having to have a user account created for
    them.  This is intended only for testing, and to make the installation
    go a bit smoother.  You should remove them before moving into a
    production environment.

    Remove anonymous users? [Y/n] y
     ... Success!

    Normally, root should only be allowed to connect from 'localhost'.  This
    ensures that someone cannot guess at the root password from the network.

    Disallow root login remotely? [Y/n]
     ... Success!

    By default, MariaDB comes with a database named 'test' that anyone can
    access.  This is also intended only for testing, and should be removed
    before moving into a production environment.

    Remove test database and access to it? [Y/n]
     - Dropping test database...
     ... Success!
     - Removing privileges on test database...
     ... Success!

    Reloading the privilege tables will ensure that all changes made so far
    will take effect immediately.

    Reload privilege tables now? [Y/n]
     ... Success!

    Cleaning up...

    All done!  If you've completed all of the above steps, your MariaDB
    installation should now be secure.

    Thanks for using MariaDB!
