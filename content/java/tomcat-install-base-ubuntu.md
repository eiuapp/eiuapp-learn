---
title: "Tomcat Install Base Ubuntu"
date: 2018-06-12T20:51:29+08:00
draft: false
tags: ["ubuntu", "install", "tomcat"]
categories: ["tomcat"]
subtitle: "ubuntu安装tomcat"
descriptions: ""
bigimg:
---


## Env

- os: ubuntu16
- java: 
```
root@ip-172-31-28-68:/home/ubuntu/java/sotre_app# java  -version
openjdk version "1.8.0_171"
OpenJDK Runtime Environment (build 1.8.0_171-8u171-b11-0ubuntu0.16.04.1-b11)
OpenJDK 64-Bit Server VM (build 25.171-b11, mixed mode)
root@ip-172-31-28-68:/home/ubuntu/java/sotre_app#
```
- nodejs: v8.11.2
- tomcat: apache-tomcat-8.5.31
- java-jre: openjdk-8-jdk

## Step

```
wget https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.31/bin/apache-tomcat-8.5.31.tar.gz
ls
tar zxvf apache-tomcat-8.5.31.tar.gz
sudo mv apache-tomcat-8.5.31 /opt/
sudo ln -s /opt/apache-tomcat-8.5.31/ /opt/tomcat8
/opt/tomcat8/bin/startup.sh
curl http://127.0.0.1:8080/
```
## Ref

- https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.31/bin/
- https://www.cnblogs.com/EasonJim/p/7202844.html