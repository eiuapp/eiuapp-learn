---
title: "Nginx Faq 2"
date: 2018-03-02T15:04:12+08:00
draft: false
tags: ["nginx"]
categories: ["nginx"]
subtitle: ""
descriptions: ""
bigimg:
---

nginx txt,json文件 中文乱码

## nginx 中 txt,json文件 中文乱码

在 /etc/nginx/conf.d/default.conf 中找到 `location / {` ,然后加入

    charset utf-8;
    charset_types text/html

如：

    server {
        listen       8000;
        server_name  localhost;

        #default_type 'text/html';
        #charset utf-8;
        #charset koi8-r;
        #access_log  /var/log/nginx/log/host.access.log  main;

        location / {
            charset utf-8;
            charset_types text/html application/json;
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Credentials' 'true';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
