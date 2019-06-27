---
title: "R Rstudio Vnc Install Centos"
date: 2018-02-03T14:38:19+08:00
draft: false
tags: ["R", "Rstudio", "install"]
categories: ["R"]
subtitle: ""
descriptions: ""
bigimg:
---

R, Rstudio 在 centos 下安装, 并可通过端口访问

```
cd
yum install scp
ls
ifconfig
ls
yum install R
yum install gcc gcc-c++ -y
vncserver
R
sudo systemctl stop firewalld.service
yum -y install tigervnc-server tigervnc
ll /lib/systemd/system/vncserver@.service
cp /lib/systemd/system/vncserver@.service /lib/systemd/system/vncserver@:1.service
vi /lib/systemd/system/vncserver@.service
systemctl daemon-reload
systemctl enable vncserver@:1.service
systemctl start vncserver@:1.service
cat /etc/sysconfig/iptables
sudo systemctl stop firewalld.service
service vncserver restart
rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
yum install epel-release
yum install gcc gcc-c++ -y
yum install gcc-gfortran -y
yum install readline-devel -y
yum install libXt-devel -y
yum install fonts-chinese tcl tcl-devel tclx tk tk-devel -y
yum install mesa-libGLU mesa-libGLU-devel -y
yum install -y openssl openssl-devel
sudo yum clean all
sudo yum install R -y
R
mkdir /home/conan/R/Rserve -p
vi /etc/Rserv.conf
vi /home/conan/R/RServe/source.R
R CMD Rserve --RS-settings
yum install -y openssl openssl-devel
R
yum install php-mssql -y
sudo yum install iodbc
R CMD Rserve --RS-enable-remote
netstat -ntlp | grep Rserve
sudo systemctl stop firewalld.service
R CMD Rserve --RS-enable-remote
netstat -ntlp | grep Rserve
sudo yum install freetds freetds-devel -y
sudo yum install unixODBC unixODBC-devel libtool-ltdl libtool-ltdl-devel -y
rpm -aq |grep unixODBC
vi /etc/freetds.conf
tsql -S stock -U zyl -P "zyl&jlch"
 tsql -S MYSQLSERVER -U cdn -P cdncdn
whereis libtdsodbc.so
R
vi /etc/Rserv.conf
whereis libtdsS.so
vi /etc/odbcinst.ini
vi /etc/odbc.ini
isql -v mssql cdn cdncdn
yum install samba samba-client samba-common -y
mv /etc/samba/smb.conf /etc/samba/smb.conf.bak
vi /etc/samba/smb.conf
mkdir -p /samba/anonymous
systemctl enable smb.service
systemctl enable nmb.service
systemctl restart smb.service
systemctl restart nmb.service
R
systemctl restart nmb.service
firewall-cmd --permanent --zone=public --add-service=samba
firewall-cmd --reload
ls -l
cd /samba
chmod -R 0755 anonymous/
chown -R nobody:nobody anonymous/
ls -l anonymous/
chcon -t samba_share_t anonymous/
ls -l anonymous/
groupadd smbgrp
useradd srijan -G smbg
smbpasswd -a srijan
mkdir -p /samba/secured
cd /samba
chmod -R 0777 secured/
chcon -t samba_share_t secured/
vi /etc/samba/smb.conf
systemctl restart smb.service
systemctl restart nmb.service
testparm
cd /samba
chown -R srijan:smbgrp secured/
sudo yum clean all
yum update -y
ps -aux|grep Rserve
netstat -nltp|grep Rserve
R CMD Rserve --RS-settings
mkdir /home/conan/R/RServe -p
shutdown -h now
cd
yum update
ifconfig
yum install  -y xml2
yum install  -y  curl curl-devel
yum install  -y xml2
yum install  -y  curl curl-devel
 yum install libxml2 libxml2-devel -y
yum install  -y xml2
yum install  -y  curl curl-devel
shutdown -r now
R
 yum search libxml2*
sudo yum install r-base
ps -aux|grep rstudio-server
sudo systemctl stop firewalld.service
yum update -y
sudo yum install curl curl-devel -y
sudo yum install libxml2 libxml2-devel
install.packages("RCurl")
install.packages("RCurl", contriburl = "http://www.stats.ox.ac.uk/pub/RWin/bin/windows/contrib/2.15/")
R
yum install gcc-gfortran  -y
yum install gcc gcc-c++
yum install readline-devel
yum install libXt-devel
 yum install libxml2 libxml2-devel
sudo yum clean all
vmstate 1 
yum install  -y xml2
 yum install libxml2 libxml2-devel
yum install  -y  curl curl-devel
sudo yum install curl curl-devel
sudo yum -y install libxml2 libxml2-devel
sudo systemctl stop firewalld.service
sudo vi /etc/Rserv.conf
vi /home/conan/R/RServe/gljg_ans_1.R
sudo vi /etc/Rserv.conf
R CMD Rserve --RS-enable-remote
systemctl status Rserve
systemctl status RServe
vncserver
systemctl status Rserve
R CMD Rserve --RS-enable-remote
vi /home/conan/R/RServe/tingyongciku2.txt
R CMD Rserve --RS-enable-remote
ab
yum update -y
ps -aux | grep Rserve
R CMD Rserve --RS-enable-remote
ps -aux | grep Rserve
systemctl
top
pstree
systemctl status rserver
systemctl status rserver.service
systemctl status httpd.service
systemctl start httpd.service
chkconfig –list
systemctl list-unit-files –type=service
ls /etc/systemd/system/*.wants/
chkconfig –-list
systemctl
pstree
systemd-cgls
ps -aux | grep Rserve
systemd-cgls
systemctl
systemctl status session-12.scope
systemctl stop session-12.scope
R
kill -9 10535
ls
ps -aux | grep Rserve
shutdown -r now
ps -aux | grep Rserve
sudo systemctl stop firewalld.service
ps -aux | grep Rserve
systemctl status session-12.scope
history
R CMD Rserve --RS-enable-remote
systemctl status session-12.scope
history 270
systemd-cgls
ps -aux | grep Rserve
 systemd-cgls
ps -aux | grep Rserve
vmstat 1
ps -aux | grep Rserve
sudo systemctl stop firewalld.service
ps -aux | grep Rserve
vi /home/conan/R/RServe/gljg_ans_1.R
vi /home/conan/R/RServe/gljg_ans_1b.R
sudo vi /etc/Rserv.conf
ps -aux | grep Rserve 8234
kill -9 3033
R CMD Rserve --RS-enable-remote
vi /home/conan/R/RServe/gljg_ans_1b.R
vi /etc/freetds.conf
ps -aux | grep Rserve
kill -9 8234
R CMD Rserve --RS-enable-remote
vi /etc/freetds.conf
vi /home/conan/R/RServe/gljg_ans_1b.R
ps -aux | grep Rserve
kill -9 8411
R CMD Rserve --RS-enable-remote
vmstat 1
history
```