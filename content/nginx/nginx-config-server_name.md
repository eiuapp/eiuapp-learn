---
title: "Nginx Config Server_name"
date: 2018-06-12T18:11:31+08:00
draft: false
tags: ["nginx", "godaddy", "DNS"]
categories: ["nginx"]
subtitle: "nginx 配合 godaddy 完成 3级域名 "
descriptions: ""
bigimg:
---

nginx 配合 godaddy 完成 3级域名 

原理：godaddy, 配置3级域名解析，转到nginx配置中的 server_name

## Env
- os: ubuntu16
- nginx: nginx/1.10.3 (Ubuntu)
- 2级域名: bitzone.space
- 3级域名: store1.bitzone.space
- godaddy

## Step

### 配置nginx

在 nginx 的 sites-enabled 中配置
```
root@ip-172-31-28-68:~# cat sites-enabled/store1
server {
    listen       80;
    server_name  store1.bitzone.space;
    client_max_body_size 80m;

    location / {
        index index.html;
	    root /home/ubuntu/html/code_commit_vac_competition/APP;
        #root /home/dev/completition/code_commit_competition/code_commit_competition_food_blockChainCheck;
    }

}
```

### 配置godaddy

打开相应地址(2级域名: bitzone.space)的DNS管理

增加
```
A	store1	52.78.79.111	600 秒
```

### 确认

浏览器访问`http://store1.bitzone.space/index.html` 就访问到了 `http://52.78.79.111:80` , 根据nginx配置就知道， 拿到了 `52.78.79.111:/home/ubuntu/html/code_commit_vac_competition/APP/index.html`这里的资源

### 新加记录

同理，我们可以增加 store2, 成 `http://store2.bitzone.space/index.html`

nginx
```
root@ip-172-31-28-68:~# cat sites-enabled/store2
server {
    listen       80;
    server_name  store2.bitzone.space;
    client_max_body_size 80m;

    location / {
        index index.html;
	root /home/ubuntu/html/code_commit_vac_competition/competitionA;
    }

}
root@ip-172-31-28-68:~#
```

godaddy
增加
```
A	store2	52.78.79.111	600 秒
```



