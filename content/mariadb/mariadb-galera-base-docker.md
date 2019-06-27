---
title: "Mariadb Galera Base Docker"
date: 2018-03-02T10:31:09+08:00
draft: false
tags: ["mariadb", "docker"]
categories: ["mariadb"]
subtitle: ""
descriptions: ""
bigimg:
---

docker之docker-mariadb-galera各种策略汇总

## 使用 `FROM mariadb:10.1` 的几个docker

* https://github.com/EgoAleSum/mariadb-cluster

http://withblue.ink/2016/03/09/galera-cluster-mariadb-coreos-and-docker-part-1.html

Coreos + any public cloud (Azure, AWS, Google, etc)

* https://github.com/dial-once/docker-mariadb-galera

使用 Docker Cloud/Docker Compose YML

* https://github.com/toughIQ/docker-mariadb-cluster

Swarm

Consider this a POC and not a production ready system!

Built for use with Docker 1.12.1+ in Swarm Mode

* https://github.com/panubo/docker-mariadb-galera

Galera Arbitrator (aka garbd)


## end
