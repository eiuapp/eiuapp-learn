---
title: "Mariadb Galera Cluster on Coreos With Docker Ref EgoAleSum"
date: 2018-04-03T16:02:27+08:00
draft: true
tags: ["mariadb", "galera", "cluster", "coreos", "docker"]
categories: ["mariadb"]
subtitle: ""
descriptions: ""
bigimg:
---

Mariadb Galera Cluster on Coreos With Docker

## step


开端口:

```
sudo firewall-cmd --permanent --add-port=4567/udp;
sudo firewall-cmd --permanent --add-port=4567/tcp; 
sudo firewall-cmd --permanent --add-port=4568/tcp; 
sudo firewall-cmd --permanent --add-port=4444/tcp; 
sudo firewall-cmd --permanent --add-port=3306/tcp; 
sudo firewall-cmd --reload; sudo firewall-cmd --permanent --list-port
```

检查mysql 是不是起来了:

```
sudo yum install -y telnet
telnet 127.0.0.1 22
Telnet 127.0.0.1 3306
```
检查日志

```
docker logs -f mariadb-container-0
```

单独启动

```
docker run \
          --name mariadb-container-0 \
	  -d \
          -v /opt/mysql.conf.d:/etc/mysql/conf.d \
          -v /data/mariadb_galera/data:/var/lib/mysql \
          -e MYSQL_INITDB_SKIP_TZINFO=yes \
          -e MYSQL_ROOT_PASSWORD=my-secret-pw \
          -p 3306:3306 \
          -p 4567:4567/udp \
          -p 4567-4568:4567-4568 \
          -p 4444:4444 \
          mariadb:10.1 \
          --wsrep-new-cluster \
          --wsrep_node_address=10.10.12.17
```

```
docker run \
          --name mariadb-container-1 \
	 -d \
          -v /opt/mysql.conf.d:/etc/mysql/conf.d \
          -v /data/mariadb_galera/data:/var/lib/mysql \
          -p 3306:3306 \
          -p 4567:4567/udp \
          -p 4567-4568:4567-4568 \
          -p 4444:4444 \
          mariadb:10.1 \
          --wsrep_node_address=10.10.12.18
```
## df

```
docker run -d --net host --name galera \
  -e WSREP_NODE_ADDRESS=$WSREP_NODE_ADDRESS \
  -e WSREP_CLUSTER_ADDRESS=$WSREP_CLUSTER_ADDRESS \
  -e MYSQL_ROOT_PASSWORD=mysql_root_password \
  -p 3306:3306 \
  -p 4567:4567/udp \
  -p 4567-4568:4567-4568 \
  -p 4444:4444 \
  -v /mnt/data/galera.service/mysql:/var/lib/mysql:Z \
  panubo/mariadb-galera \
    mysqld \
    --wsrep-new-cluster
```
## ref

- [github.com/EgoAleSum/mariadb-cluster](https://github.com/EgoAleSum/mariadb-cluster)