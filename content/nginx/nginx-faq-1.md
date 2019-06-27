---
title: "Nginx Faq 1"
date: 2018-03-02T15:03:32+08:00
draft: false
tags: ["nginx"]
categories: ["nginx"]
subtitle: ""
descriptions: ""
bigimg:
---



nginx 跨域访问

* nginx 解决跨域访问的问题
* 跨域造成session丢失

## nginx 解决跨域访问的问题。

https://michielkalkman.com/snippets/nginx-cors-open-configuration.html

http://enable-cors.org/server_nginx.html

1. 所有网页都实现跨域

把下面的代码，放到 ，配置文件 /etc/nginx/conf.d/default.conf 中的 location / { 内

    add_header 'Access-Control-Allow-Origin' '*';
    add_header 'Access-Control-Allow-Credentials' 'true';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';


2. 部分url（下以 /ok 为例） 请求实现跨域访问

把下面的代码，放到 ，配置文件 /etc/nginx/conf.d/default.conf 中 的 location /ok { 内


    #
    # Wide-open CORS config for nginx
    #
    location / {
         if ($request_method = 'OPTIONS') {
            add_header 'Access-Control-Allow-Origin' '*';
            #
            # Om nom nom cookies
            #
            add_header 'Access-Control-Allow-Credentials' 'true';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            #
            # Custom headers and headers various browsers *should* be OK with but aren't
            #
            add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
            #
            # Tell client that this pre-flight info is valid for 20 days
            #
            add_header 'Access-Control-Max-Age' 1728000;
            add_header 'Content-Type' 'text/plain charset=UTF-8';
            add_header 'Content-Length' 0;
            return 204;
         }
         if ($request_method = 'POST') {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Credentials' 'true';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
         }
         if ($request_method = 'GET') {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Credentials' 'true';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
         }
    }


## 跨域造成session丢失

https://segmentfault.com/q/1010000002905817

重点参考下面这个地方：

    这个配置在不少地方应该都能找到，不同的主要是两点：
    1。response.setHeader("Access-Control-Allow-Credentials","true"); //是否支持cookie跨域
    2。response.setHeader("Access-Control-Allow-Origin", request.getHeader("Origin"));

    首先，配置了allow-credentials之后，如果allow-origin设为*，跨域时会报错说因为允许credentials，origin不能设为通配*，那所以设为简单的某个domain也是可以的，这种写法应该就是达到了任意domain都可以的效果吧。

    然后angular部分也要设定个东西，举个栗子~

解决方法：

填加下面的代码到 服务端代码 如：limitup/route/index.js中。

    app.all('*', function(req, res, next) {
	    res.header('Access-Control-Allow-Origin', req.headers.origin);
        res.header('Access-Control-Allow-Methods', 'GET, POST');
        res.header('Access-Control-Allow-Headers', 'Content-Type');
        res.header('Access-Control-Allow-Credentials', 'true');
        next();
    });


## end
