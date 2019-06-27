---
title: "Privoxy Mac"
date: 2018-07-15T14:16:28+08:00
draft: false
tags: ["mac", "privoxy", "shadowsocks", "network"]
categories: ["privoxy"]
subtitle: "mac osx 下面通过 privoxy 把shadowsocks 转换成http代理"
descriptions: ""
bigimg:
---

mac osx 下面通过 privoxy 把shadowsocks 转换成http代理

## Env

- os: mac

## Step

```
➜  ~ brew install privoxy
...
...
To have launchd start privoxy now and restart at login:
  brew services start privoxy
Or, if you don't want/need a background service you can just run:
  privoxy /usr/local/etc/privoxy/config
==> Summary
🍺  /usr/local/Cellar/privoxy/3.0.26: 99 files, 2.2MB
==> Caveats
==> privoxy
To have launchd start privoxy now and restart at login:
  brew services start privoxy
Or, if you don't want/need a background service you can just run:
  privoxy /usr/local/etc/privoxy/config
➜  ~
```

修改配置文件`/usr/local/etc/privoxy/config`

```
➜  ~ vi /usr/local/etc/privoxy/config
➜  ~ tail -1 /usr/local/etc/privoxy/config
forward-socks5 / 127.0.0.1:1080 .
➜  ~
```

启动privoxy服务

```
➜  ~ brew services start privoxy
==> Tapping homebrew/services
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-services'...
remote: Counting objects: 14, done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 14 (delta 0), reused 8 (delta 0), pack-reused 0
Unpacking objects: 100% (14/14), done.
Tapped 1 command (43 files, 55.2KB).
==> Successfully started `privoxy` (label: homebrew.mxcl.privoxy)
➜  ~ which privoxy
privoxy not found
➜  ~ 
```

重新启动privoxy服务

```
➜  ~ brew services start privoxy
Service `privoxy` already started, use `brew services restart privoxy` to restart.
➜  ~ brew services restart privoxy
Stopping `privoxy`... (might take a while)
==> Successfully stopped `privoxy` (label: homebrew.mxcl.privoxy)
==> Successfully started `privoxy` (label: homebrew.mxcl.privoxy)
➜  ~
```

验证已启动

```
➜  ~ ps aux  | grep privoxy
tomtsang         79888   0.1  0.0  4276968    896 s006  S+    2:34PM   0:00.01 grep --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn privoxy
tomtsang         79524   0.0  0.0  4296096   1192   ??  S     2:20PM   0:00.03 /usr/local/Cellar/privoxy/3.0.26/sbin/privoxy --no-daemon /usr/local/etc/privoxy/config
➜  ~ netstat -an | grep 8118
tcp4       0      0  127.0.0.1.8118         *.*                    LISTEN
➜  ~
➜  ~ telnet 127.0.0.1 8118
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
'^]
telnet> quit
Connection closed.
➜  ~ 
```

加入环境变量

```
➜  ~ tail -17 ~/.zshrc
### proxy
function proxy_off(){
    unset HTTP_PROXY
    unset HTTPS_PROXY
    unset http_proxy
    unset https_proxy
    echo -e "已关闭代理"
}

function proxy_on() {
    HTTP_PROXY=http://127.0.0.1:8118
    http_proxy=${HTTP_PROXY}
    HTTPS_PROXY=${HTTP_PROXY}
    https_proxy=${HTTP_PROXY}
    export HTTP_PROXY HTTPS_PROXY http_proxy https_proxy
    echo -e "已开启代理"
}
➜  ~ source .zshrc
➜  ~ 
```

验证科学上网成功

```
➜  ~ proxy_on
已开启代理
➜  ~ export | grep proxy
http_proxy=http://127.0.0.1:8118
https_proxy=http://127.0.0.1:8118
➜  ~ gvm listall

gvm gos (available)

   go1
   go1.0.1
   go1.0.2
   go1.0.3
   go1.1
   go1.1r
   ...
➜  ~ 
```

关闭科学上网
```
➜  ~ proxy_off
已关闭代理
➜  ~ gvm listall

gvm gos (available)

fatal: unable to access 'https://go.googlesource.com/go/': Failed to connect to go.googlesource.com port 443: Operation timed out

➜  ~ 
```
## Ref

- https://www.privoxy.org/
- http://www.fyhqy.com/post-383.html
- https://blog.csdn.net/KimBing/article/details/70943325
- https://blog.csdn.net/biyongyao/article/details/78636505