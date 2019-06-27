+++
title = "To Completly remove Mysql from Ubuntu"
date = 2019-02-13T00:00:00-08:00
lastmod = 2019-02-13T18:09:25-08:00
tags = ["mysql"]
categories = ["mysql"]
draft = false
+++

To Completly remove Mysql from Ubuntu

## env

- os: ubuntu
- mysql: 5.7
- mariadb: 10.3

## step


```shell
sudo apt-get remove --purge mysql-server mysql-client mysql-common 
sudo apt-get autoremove
sudo apt-get autoclean
```

如果有安装过 mariadb

```shell
sudo apt-get remove --purge mariadb*
sudo apt-get autoremove
sudo apt-get autoclean
```

after this, if you are having issues with re installing, Try to remove Mysql files in :

```shell
sudo rm -rf /var/lib/mysql
```

## ref
- https://stackoverflow.com/questions/25244606/completely-remove-mysql-ubuntu-14-04-lts
