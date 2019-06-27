---
title: "Nginx Seo"
date: 2018-02-27T11:08:59+08:00
draft: false
tags: ["nginx", "seo"]
categories: ["nginx"]
subtitle: ""
descriptions: ""
bigimg:
---

#### [seo 优化去掉html 页面的后缀 .html](http://blog.csdn.net/danmo598/article/details/54947321)

```
ubuntu@VM-0-12-ubuntu:/etc/nginx/conf.d$ cat 8766.conf
server {
    listen       8766;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    # seo 优化去掉html 页面的后缀 .html
    if (!-f $request_filename){
        set $rule_0 1$rule_0;
    }
    if ($rule_0 = "1"){
        rewrite ^/([^\.]+)$ /$1.html last;
    }

    location / {
        root   /home/ubuntu/registry/tomtsang-rootsongjc-cheatsheet/_site;
        index  index.html index.htm;
        # proxy_pass   http://127.0.0.1:4001;
    }
    ...
```