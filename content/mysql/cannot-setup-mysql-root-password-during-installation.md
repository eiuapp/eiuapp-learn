+++
title = "Unable to set password for the MariaDB ‘root’ user"
date = 2019-02-13T00:00:00-08:00
lastmod = 2019-02-13T18:09:25-08:00
tags = ["mysql"]
categories = ["mysql"]
draft = false
+++

## env

在安装 mysql 或者 mariadb 过程中，设置完 root 密码后，报出类似下面错误：

```text
Unable to set password for the MariaDB "root" user    

An error occurred while setting the password for the MariaDB administrative user. This may have happened because the account already has a password, or because of a communication problem with the MariaDB server.   

You should check the account's password after the package installation.  

Please read the /usr/share/doc/mariadb-server-10.2/README.Debian file for more information.  
```

## step

说明，之前的安装，没有 completly remove。怎么remove, 看 [这里](./remove-mysql-mariadb-from-ubuntu/)

