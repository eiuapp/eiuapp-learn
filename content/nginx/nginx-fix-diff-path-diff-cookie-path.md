
解决同域名不同路径下形成不同session cookie, 从而使得不同的帐户密码登录

## targit

我的最终目的是要让 

- http://192.168.168.137:8081/ 使用 一个帐号密码(admin:adminsz189)登录
- http://192.168.168.137:8081/admin/ 使用 另一个帐号密码(admin:admin)登录

## env

![ip-diff-path-same-jsessionid-domain-path](https://user-images.githubusercontent.com/39667631/59911628-ead59c00-9446-11e9-864b-4bf5e1741cbc.png)


参考 [解决nginx proxy_pass反向代理cookie,session丢失的问题](https://www.jianshu.com/p/34abe7eb6f0b)

## 为什么cookie 会丢失？


比如说一个没有经过代理的地址 ： http://127.0.0.1/project cookie_path：/project

如果按照第二种方式代理 那么地址就是 ： http://127.0.0.1/proxy_path cookie_path: /proxy_path

如果cookie_path与地址栏上的path不相符游览器就不会接受这个cookie，自然session就失效了

## proxy_cookie_path 的用法

proxy_cookie_path 的作用是用来改变cookie的路径

语法： proxy_cookie_path path replacement;  path就是你要替换的路径  replacement 就是要替换的值

详情可以去nginx 官网看看 [传送门](http://nginx.org/en/docs/http/ngx_http_proxy_module.html?&_ga=1.161910972.1696054694.1422417685#proxy_cookie_path)

下面是可能的三种情况

1. host、端口转换，cookie不会丢失

```
    location /project {
        proxy_pass   http://127.0.0.1:8080/project;
    }
```

2. 路径也变化，则需要设置cookie的路径转换

```
    location /proxy_path {
        proxy_pass   http://127.0.0.1:8080/project;
        proxy_cookie_path  /project /proxy_path;
    }
```

3. 直接代理本地端口

```
    location /proxy_path {
        proxy_pass   http://127.0.0.1:8080/;
        proxy_cookie_path  /project /proxy_path; # project 为你的项目名 也可用变量代替
    }
```

## 实践

到这里，我们知道，我的实战环境是第2种，设置cookie的路径转换

修改 /admin/ 的 cookie path

```
	location /admin/{
			proxy_pass http://127.0.0.1:3021/;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_cookie_path  / /admin;
   }
```

最后效果如下：

![](https://raw.githubusercontent.com/eiuapp/img/master/img/nginx-change-cookie-path.png?token=AJOUPL4JNZOH5MRSZ5CJS5C5CCSRO)

## （可跳过）弯路

因为这里是使用的是 express-session 所以，到 

https://github.com/expressjs/session

然后，设置 cookie.path 如下：

```
app.use(session({
  store: new RedisStore({
    prefix: "admin:",
    host: process.env.REDIS_URL,
    db: process.env.REDIS_DB
  }),
  secret: pkgInfo.description,
  resave: false,
  saveUninitialized: true,
  cookie: {
  maxAge: 3600 * 1000 * 1000,
  httpOnly: true,
  path: "/admin/"
  }
}))
```

发现服务会报错。找原因。

找到 /node_modules/_express-session@1.16.1@express-session/index.js 下的

```
    // pathname mismatch
    var originalPath = parseUrl.original(req).pathname || '/'
    if (originalPath.indexOf(cookieOptions.path || '/') !== 0) return next();
```

这个地方，我们console.log 一下。

```
-----》 originalPath /GET_LIST/device_rent_manage
----->   cookieOptions.path undefined
```

发现，originalPath 是 "/GET_LIST/device_rent_manage", 而不是我们以为的 "/admin/GET_LIST/device_rent_manage"

所以，这样来说，我们的 cookie path 不是 "/admin" 而是 "/" .

回想一下，这个是合理的，因为nginx通过 "/admin" 把端口转发至其它端口了，所以，实际处理进程，是不知道 "/admin" 的存在的。

那么，也就意味着，程序修改不了 cookie 的path.

### 思路

- 修改 cookie 的 path
- 修改 cookie 的 Domain

### 修改 cookie 的 Domain

要把 /admin 放在二级域名下. 成本比较高。


### 修改 cookie 的 path

程序修改不了 cookie.path ，但是其它应用可以（比如nginx）

google: nginx proxy_cookie_path 

找到 
- [解决nginx proxy_pass反向代理cookie,session丢失的问题](https://www.jianshu.com/p/34abe7eb6f0b)
- [解决nginx使用proxy_pass反向代理时,session丢失的问题](https://www.cnblogs.com/zangdalei/p/6021352.html)

如上配置就解决了。

## 关于 cookie, session 的其它参考

- [node.js操作Cookie](https://www.cnblogs.com/rubylouvre/archive/2012/08/19/2645644.html)
- [Cookie和Session详解](https://www.cnblogs.com/linguoguo/p/5106618.html)
- [express-session](https://github.com/expressjs/session/)
- [session_set_cookie_params](https://www.php.net/manual/zh/function.session-set-cookie-params.php)
 
