---
title: "Ngrok Server Install(搭建自己的ngrok服务器)"
date: 2018-04-19T15:18:37+08:00
draft: false
tags: ["ngrok", "install", "network"]
categories: ["ngrok"]
subtitle: ""
descriptions: ""
bigimg:
---

搭建自己的ngrok服务器

## env

### 服务端
- 机器: 腾讯云主机一台
- IP: 111.230.153.251
- OS: ubuntu server 16.04.1
### 客户端

- 机器: 腾讯云主机一台
- IP: 192.168.31.106
- OS: ubuntu desktop 16.04.3

## step

主要就是参考了 [搭建 ngrok 服务实现内网穿透](https://hkshop.club/post/self-hosted-ngrokd.html) 一文

### 编译 ngrok

编译我是在服务端完成的

首先装必要的工具：
```
sudo apt-get install build-essential golang mercurial git
```
获取 ngrok 源码：

```
git clone https://github.com/inconshreveable/ngrok.git ngrok
### 请使用下面的地址，修复了无法访问的包地址
git clone https://github.com/tutumcloud/ngrok.git ngrok
cd ngrok
```
生成并替换源码里默认的证书，注意域名修改为你自己的。（之后编译出来的服务端客户端会基于这个证书来加密通讯，保证了安全性）

```
NGROK_DOMAIN="hkshop.club"

openssl genrsa -out base.key 2048
openssl req -new -x509 -nodes -key base.key -days 10000 -subj "/CN=$NGROK_DOMAIN" -out base.pem
openssl genrsa -out server.key 2048
openssl req -new -key server.key -subj "/CN=$NGROK_DOMAIN" -out server.csr
openssl x509 -req -in server.csr -CA base.pem -CAkey base.key -CAcreateserial -days 10000 -out server.crt

cp base.pem assets/client/tls/ngrokroot.crt
```
开始编译：
```
sudo make release-server release-client
```
如果一切正常，ngrok/bin 目录下应该有 ngrok、ngrokd 两个可执行文件。


### 服务端

前面生成的 ngrokd 就是服务端程序了，指定证书、域名和端口启动它（证书就是前面生成的，注意修改域名）：
```
sudo ./bin/ngrokd -tlsKey=server.key -tlsCrt=server.crt -domain="hkshop.club" -httpAddr=":8081" -httpsAddr=":8082"
```
到这一步，ngrok 服务已经跑起来了，可以通过屏幕上显示的日志查看更多信息。httpAddr、httpsAddr 分别是 ngrok 用来转发 http、https 服务的端口，可以随意指定。ngrokd 还会开一个 4443 端口用来跟客户端通讯（可通过 -tunnelAddr=":xxx" 指定），如果你配置了 iptables 规则，需要放行这三个端口上的 TCP 协议。

现在，通过 https://hkshop.club:8081 和 https://hkshop.club:8082 就可以访问到 ngrok 提供的转发服务。为了使用方便，建议把域名泛解析到 VPS 上，这样能方便地使用不同子域转发不同的本地服务。

#### 域名配置泛解析

![域名配置泛解析](https://res.cloudinary.com/dmtixvmgt/image/upload/v1524265009/yuming-setting-up_h8n45j.jpg)

可以通过 ping 来检测
```
➜  ~ ping hkshop.club
PING hkshop.club (111.230.153.251): 56 data bytes
64 bytes from 111.230.153.251: icmp_seq=0 ttl=53 time=12.250 ms
^C
--- hkshop.club ping statistics ---
1 packets transmitted, 1 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 12.250/12.250/12.250/0.000 ms
➜  ~ ping pub.hkshop.club
PING pub.hkshop.club (111.230.153.251): 56 data bytes
64 bytes from 111.230.153.251: icmp_seq=0 ttl=53 time=11.128 ms
64 bytes from 111.230.153.251: icmp_seq=1 ttl=53 time=12.798 ms
^C
--- pub.hkshop.club ping statistics ---
2 packets transmitted, 2 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 11.128/11.963/12.798/0.835 ms
➜  ~ ping abc.hkshop.club
PING abc.hkshop.club (111.230.153.251): 56 data bytes
64 bytes from 111.230.153.251: icmp_seq=0 ttl=53 time=12.531 ms
64 bytes from 111.230.153.251: icmp_seq=1 ttl=53 time=11.356 ms
^C
--- abc.hkshop.club ping statistics ---
2 packets transmitted, 2 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 11.356/11.944/12.531/0.587 ms
➜  ~
```
ping 出来都是同一个IP, 说明泛解析配置成功.

我给 hkshop.club 做了泛解析，随便访问一个子域，如：http://pub.hkshop.club:8081，可以看到这样一行提示：

`Tunnel pub.hkshop.club:8081 not found`
这说明万事俱备，只差客户端来连了。

### 客户端

因为我的客户端也是 ubuntu 所以, 我这里直接使用之前编译的 ngrok 文件就可以了.

> 如果要把 linux 上的服务映射出去，客户端就是前面生成的 ngrok 文件。但我用的是 Mac，需要指定环境变量再编一次：
> ```
> sudo GOOS=darwin GOARCH=amd64 make release-server release-client
> ```
> 这样在 ngrok/bin 目录下会多出来一个 darwin_amd64 目录，这里的 ngrok 文件就可以拷到 Mac 系统用了。

```
tom@udvbud-basic-a:~/ngrok-cli$ pwd
/home/tom/ngrok-cli
tom@udvbud-basic-a:~/ngrok-cli$ ls
ngrok  ngrok.cfg
tom@udvbud-basic-a:~/ngrok-cli$ cat ngrok.cfg
server_addr: "hkshop.club:4443"
trust_host_root_certs: false
tom@udvbud-basic-a:~/ngrok-cli$ ./ngrok -subdomain=pub -proto=http -config=ngrok.cfg 80
```
写一个简单的配置文件，随意命名如 ngrok.cfg：
```
server_addr: "hkshop.club:4443"
trust_host_root_certs: false
```
指定子域、要转发的协议和端口，以及配置文件，运行客户端：
```
./ngrok -subdomain=pub -proto=http -config=ngrok.cfg 80
```
不出意外可以看到这样的界面，这说明已经成功连上远端服务了：

![ngrok_client](https://st.imququ.com/i/webp/static/uploads/2015/04/ngrok_client.png.webp)

现在再访问 http://pub.hkshop.club:8081，访问到的已经是我本机 80 端口上的服务了。

### 管理界面

> 管理界面, 只能通过`127.0.0.1:4040`打开,而不能通过`192.168.31.106:4040`打开, 也就意味着, 应该把 ngrok 客户端放在一台有 desktop 的机器上.

```
tom@udvbud-basic-a:~$ telnet 127.0.0.1 4040
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
^]

telnet> quit
Connection closed.
tom@udvbud-basic-a:~$ telnet 192.168.31.106 4040
Trying 192.168.31.106...
telnet: Unable to connect to remote host: Connection refused
tom@udvbud-basic-a:~$
```


上面那张 ngrok 客户端运行界面截图中，有一个 Web Interface 地址，这是 ngrok 提供的监控界面。通过这个界面可以看到远端转发过来的 http 详情，包括完整的 request/response 信息，非常方便。

![ngrok_manager](https://st.imququ.com/i/webp/static/uploads/2015/04/ngrok_manager.png.webp)

实际上，由于 ngrok 可以转发 TCP，所以还有很多玩法，原理都一样，这里就不多写了。

### 服务端设置为系统程序，并后台运行。

服务器在运行ngrokd时，如果关闭会话窗口，会导致服务中断，很显然这不是我们想要的结果，我们需要服务不断的在后台运行，当需要的时候在停止。

在/etc/systemd/system/目录下创建服务ngrok.service.

其中要根据自己的实际目录修改相对应的目录。

这样我们就可以了通过`systemctl start ngrok.service`启动服务。然后就可以愉快的玩耍了。

```
ubuntu@VM-0-12-ubuntu:/etc/systemd/system$ pwd
/etc/systemd/system
ubuntu@VM-0-12-ubuntu:/etc/systemd/system$ cat ngrok-e.service
[Unit]
Description=ngrok
After=network.target

[Service]
ExecStart=/home/ubuntu/ngrok/ngrok-e/ngrok/bin/ngrokd -tlsKey=/home/ubuntu/ngrok/ngrok-e/ngrok/server.key -tlsCrt=/home/ubuntu/ngrok/ngrok-e/ngrok/server.crt -domain="hkshop.club" -httpAddr=":8081" -httpsAddr=":8082"

[Install]
WantedBy=multi-user.target
ubuntu@VM-0-12-ubuntu:/etc/systemd/system$

ubuntu@VM-0-12-ubuntu:/etc/systemd/system$ sudo systemctl start ngrok-e.service
ubuntu@VM-0-12-ubuntu:/etc/systemd/system$ sudo systemctl status ngrok-e.service
● ngrok-e.service - ngrok
   Loaded: loaded (/etc/systemd/system/ngrok-e.service; disabled; vendor preset: enabled)
   Active: active (running) since Sat 2018-04-21 08:25:50 CST; 2s ago
 Main PID: 19361 (ngrokd)
    Tasks: 3
   Memory: 1.1M
      CPU: 3ms
   CGroup: /system.slice/ngrok-e.service
           └─19361 /home/ubuntu/ngrok/ngrok-e/ngrok/bin/ngrokd -tlsKey=/home/ubuntu/ngrok/ngrok-e/ngrok/server.key -tlsCrt=/home/ubuntu/ngrok/ngrok-e/ngrok/server.crt -domain=hkshop.club -httpAddr=:8081 -

Apr 21 08:25:50 VM-0-12-ubuntu systemd[1]: Started ngrok.
Apr 21 08:25:50 VM-0-12-ubuntu ngrokd[19361]: [08:25:50 CST 2018/04/21] [INFO] (ngrok/log.(*PrefixLogger).Info:83) [registry] [tun] No affinity cache specified
Apr 21 08:25:50 VM-0-12-ubuntu ngrokd[19361]: [08:25:50 CST 2018/04/21] [INFO] (ngrok/log.Info:112) Listening for public http connections on [::]:8081
Apr 21 08:25:50 VM-0-12-ubuntu ngrokd[19361]: [08:25:50 CST 2018/04/21] [INFO] (ngrok/log.Info:112) Listening for public https connections on [::]:8082
Apr 21 08:25:50 VM-0-12-ubuntu ngrokd[19361]: [08:25:50 CST 2018/04/21] [INFO] (ngrok/log.Info:112) Listening for control and proxy connections on [::]:4443
Apr 21 08:25:50 VM-0-12-ubuntu ngrokd[19361]: [08:25:50 CST 2018/04/21] [INFO] (ngrok/log.(*PrefixLogger).Info:83) [metrics] Reporting every 30 seconds
ubuntu@VM-0-12-ubuntu:/etc/systemd/system$ 
```

### 注意事项

#### 
客户端ngrok.cfg中server_addr后的值必须严格与-domain以及证书中的NGROK_BASE_DOMAIN相同，否则Server端就会出现如下错误日志：
```
[03/13/15 09:55:46] [INFO] [tun:15dd7522] New connection from 54.149.100.42:38252
[03/13/15 09:55:46] [DEBG] [tun:15dd7522] Waiting to read message
[03/13/15 09:55:46] [WARN] [tun:15dd7522] Failed to read message: remote error: bad certificate
[03/13/15 09:55:46] [DEBG] [tun:15dd7522] Closing
```

#### 域名一直不能访问 

如果域名一直不能访问 需要配置DNS 两个A记录 泛解析*和@这样创建的域名才能访问你的本地配置的地址

#### make release-server时发现每次都在gopkg时没反应 

注意在make release-server时发现每次都在gopkg时没反应，后来查到是Git版本太老了 ，安装git (`yum install git`).
记得按上述方法安装git前,先运行 删除旧版本(`yum remove git`)。

#### 找不到 sync.Pool

在编译生成的时候执行
```
GOOS=windows GOARCH=386 make release-server 
```
生成服务器端一切正常，可是生成客户端的时候一直报错，说找不到 sync.Pool。后来发现是GO语言软件版本太低了，使用1.3以上版本就不会有问题。

## TODO

### 把一级域名修改成二级域名

让用户通过 `***.ngrok.hkshop.club` 的方式访问内网服务

### 多个ngrok服务

## ref

- [搭建 ngrok 服务实现内网穿透](https://hkshop.club/post/self-hosted-ngrokd.html)
- [一分钟实现内网穿透（ngrok服务器搭建）](https://blog.csdn.net/zhangguo5/article/details/77848658)
- [搭建自己的ngrok服务](https://blog.csdn.net/gebitan505/article/details/48176763)
- [小米球ngrok](http://ngrok.ciqiuwl.cn/)
- [10分钟教你搭建自己的ngrok服务器](https://blog.csdn.net/yjc_1111/article/details/79353718)
- [最新图解 利用 ngrok 免费内网穿透部署 微信开发 调试环境](http://zhoudaxiaa.com:8080/?p=83)
- [如何让外网访问到本地内网-ngrok内网穿透免费服务器](http://ngrok.bob.kim/)