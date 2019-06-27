---
title: "Network Proxy Shadowsocks Server"
date: 2018-06-11T20:34:39+08:00
draft: false
tags: ["network", "proxy", "shadowssocks"]
categories: ["proxy"]
subtitle: "一键脚本搭建SS/搭建SSR服务并开启BBR加速"
descriptions: ""
bigimg:
---

## Env

- cloud: aws
- os: ubuntu16
- ip: 美国IP

## Ref

直接看下面文章就可以了
- https://github.com/flyzy2005/ss-fly
- https://www.flyzy2005.com/fan-qiang/shadowsocks/install-shadowsocks-in-one-command/#shadowsocks
## Step

```
git clone https://github.com/Flyzy2005/ss-fly
ss-fly/ss-fly.sh -i password 1024
ping www.youtube.com
```

```
修改配置文件：vim /etc/shadowsocks.json
停止ss服务：ssserver -c /etc/shadowsocks.json -d stop
启动ss服务：ssserver -c /etc/shadowsocks.json -d start
重启ss服务：ssserver -c /etc/shadowsocks.json -d restart
```

卸载ss服务


```
ss-fly/ss-fly.sh -uninstall
```
## Article

注意：在特殊时期，一般是大会期间，即使是设置好了 SS,但是还是不能科学上网。
比如，我就遇到了一次，在 上合组织峰会第十八次峰会-青岛峰会（2018年6月9日到10日）之后的，2018-06-11号，就出现了（大佬们，有没有哪位，和我类似）情况：

1. 美国IP 不能科学上网
2. 香港IP 能科学上网

还有一种可能性，IP段中的其他同志的IP搞歪东西，把我给扯到了。
GXB直接封的IP段，这个时候，IP段中的新机器也不行。

---

一键脚本搭建SS/搭建SSR服务并开启BBR加速


## Ref
- https://blog.csdn.net/ZhangAdo/article/details/50663527
- https://blog.csdn.net/f59130/article/details/74014415
- https://blog.csdn.net/amoscn/article/details/79364599
