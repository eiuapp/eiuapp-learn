---
title: "Proxy"
date: 2018-08-10T09:49:28+08:00
draft: false
tags: ["proxy", "network"]
categories: ["proxy"]
subtitle: "正确的设置代理"
descriptions: ""
bigimg:
---

正确的设置代理

## Env

- os: linux


## Step

```
root@tom:~/proxy# cat ./proxy_on.sh 
#!/bin/sh
 
# for terminal
export proxyserveraddr=127.0.0.1
export proxyserverport=8118
export HTTP_PROXY="http://$proxyserveraddr:$proxyserverport/"
export HTTPS_PROXY="https://$proxyserveraddr:$proxyserverport/"
export FTP_PROXY="ftp://$proxyserveraddr:$proxyserverport/"
export SOCKS_PROXY="socks://$proxyserveraddr:$proxyserverport/"
export NO_PROXY="localhost,127.0.0.1,localaddress,.localdomain.com,200.200..;11.11.0.0;"
export http_proxy="http://$proxyserveraddr:$proxyserverport/"
export https_proxy="https://$proxyserveraddr:$proxyserverport/"
export ftp_proxy="ftp://$proxyserveraddr:$proxyserverport/"
export socks_proxy="socks://$proxyserveraddr:$proxyserverport/"
export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com,200.200..;11.11.0.0,10.88..;"
 
# for chrome,firefox
gsettings set org.gnome.system.proxy ignore-hosts "['localhost', '11.11.0.0/16', '200.200.0.0/16', '*.localdomain.com', '10.88.0.0/16', '10.88.88.116' ]"
 
# for apt-get
cat <<-EOF| sudo tee /etc/apt/apt.conf
Acquire::http::proxy "http://$proxyserveraddr:$proxyserverport/";
Acquire::https::proxy "https://$proxyserveraddr:$proxyserverport/";
Acquire::ftp::proxy "ftp://$proxyserveraddr:$proxyserverport/";
Acquire::socks::proxy "socks://$proxyserveraddr:$proxyserverport/";
EOF
```

```
root@tom:~/proxy# cat proxy_off.sh 
#!/bin/sh
unset proxyserveraddr
unset proxyserverport
unset HTTP_PROXY
unset HTTPS_PROXY
unset FTP_PROXY
unset SOCKS_PROXY
unset NO_PROXY
unset http_proxy
unset https_proxy
unset ftp_proxy
unset socks_proxy
unset no_proxy
gsettings reset org.gnome.system.proxy ignore-hosts
echo -n ""|sudo tee /etc/apt/apt.conf
root@tom:~/proxy# 
```

## Ref

- http://www.cnblogs.com/scue/p/3891879.html
- http://www.crazyfairy.cn/archives/257