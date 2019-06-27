+++
title = "批量删除docker images"
date = 2019-01-27T00:00:00-08:00
lastmod = 2019-01-27T19:43:41-08:00
tags = ["docker", "images"]
categories = ["docker"]
draft = false
weight = 3003
+++

```shell
docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker stop
docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker rm
docker images|grep none|awk '{print $3 }'|xargs docker rmi
```


## Ref {#ref}

-   <https://blog.csdn.net/p656456564545/article/details/50097145>
