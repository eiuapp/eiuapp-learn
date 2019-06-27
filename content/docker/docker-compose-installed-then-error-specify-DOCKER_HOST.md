+++
title = "docker-compose安装后报\"specify DOCKER_HOST\"错误"
date = 2019-01-27T00:00:00-08:00
lastmod = 2019-01-27T19:40:10-08:00
tags = ["docker", "docker-compose"]
categories = ["docker"]
draft = false
weight = 3002
+++

按照官网 <https://docs.docker.com/compose/install/> 安装完 docker-compose 后，要重
新进入一下用户(比如切换为root，再次切换为user)，配置才生效。

否则会报出下面的错误：

```shell
ERROR: Couldn’t connect to Docker daemon at http+docker://localunixsocket - is it running?

If it’s at a non-standard location, specify the URL with the DOCKER_HOST environment variable.
```


## Ref {#ref}

-   <https://blog.csdn.net/xiojing825/article/details/79494408>
