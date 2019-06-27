---
title: "Nginx Servername"
date: 2018-06-25T14:13:09+08:00
draft: false
tags: ["nginx"]
categories: ["nginx"]
subtitle: "nginx中servername的作用"
descriptions: ""
bigimg:
---

nginx中servername的作用

## Env

- os: ubuntu16 
- 外网ip: 13.209.68.247
- domain name: link.devs1.xsl.ph

## Step

当配置了servername后，只能通过 servername 访问被代理URL

看配置 
```
root@ip-172-31-21-164:/etc/nginx/sites-enabled# cat links
server{
	listen 80;
	server_name link.devs1.xsl.ph; 
	client_max_body_size 80m;
	location /spider {
		proxy_pass http://127.0.0.1:9003/spider;
		proxy_set_header Host $host:$server_port;
		proxy_set_header X-Real-IP $remote_addr;
	}
}
root@ip-172-31-21-164:/etc/nginx/sites-enabled# /etc/init.d/nginx restart
[ ok ] Restarting nginx (via systemctl): nginx.service
root@ip-172-31-21-164:/etc/nginx/sites-enabled# 
```

请求

```
root@ip-172-31-21-164:/etc/nginx/sites-enabled# curl link.devs1.xsl.ph/spider/news/hello
hello,null.server.port==9003spring.profiles.active==testspring.datasource.url==jdbc:mysql://localhost:3306/link?useUnicode=true&characterEncoding=utf-8
root@ip-172-31-21-164:/etc/nginx/sites-enabled# 
```

但是通过其它都不行。`127.0.0.1`,`localhost`,外网IP`13.209.68.247`都不可以的。都会返回404错误。

```
root@ip-172-31-21-164:/etc/nginx/sites-enabled# curl 13.209.68.247/spider/news/hello
<html>
<head><title>404 Not Found</title></head>
<body bgcolor="white">
<center><h1>404 Not Found</h1></center>
<hr><center>nginx/1.10.3 (Ubuntu)</center>
</body>
</html>
root@ip-172-31-21-164:/etc/nginx/sites-enabled# curl localhost/spider/news/hello
<html>
<head><title>404 Not Found</title></head>
<body bgcolor="white">
<center><h1>404 Not Found</h1></center>
<hr><center>nginx/1.10.3 (Ubuntu)</center>
</body>
</html>
root@ip-172-31-21-164:/etc/nginx/sites-enabled# curl 127.0.0.1:80/spider/news/hello
<html>
<head><title>404 Not Found</title></head>
<body bgcolor="white">
<center><h1>404 Not Found</h1></center>
<hr><center>nginx/1.10.3 (Ubuntu)</center>
</body>
</html>
root@ip-172-31-21-164:/etc/nginx/sites-enabled# 
```

当然，如果想从外网访问，请配置`domain name: link.devs1.xsl.ph`的DNS管理，把`domain name: link.devs1.xsl.ph`指向本机的外网IP`13.209.68.247`。
然后通过浏览器就可以拿到结果了。

## Ref

