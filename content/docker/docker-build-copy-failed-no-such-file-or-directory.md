+++
title = "docker build时运行 COPY 时报no such file or directory 错误"
date = 2019-01-13T00:00:00+08:00
lastmod = 2019-01-13T22:55:13+08:00
tags = ["docker", "faq"]
categories = ["docker"]
draft = false
weight = 3001
+++

## env {#env}

Expected behavior
COPY command to copy a file from local to image

Actual behavior
doing a simple COPY command in Dockerfile is throwing this error when the file is in a folder (not same level as Dockerfile file)

Description

I have a Dockerfile that has the line:

COPY MyAgSourceAPI/conf/php/testsql.php _var/www_

But it causes the error:

COPY failed: stat /var/lib/docker/tmp/docker-builder918577595/MyAgSourceAPI/conf/php/testsql.php: no such file or directory


## step {#step}

有一些解决方案在  <https://github.com/docker/for-mac/issues/1922> 提出来。

我这里的解决方案是：指定 Dockerfile 路径(-f). 具体如下：

```shell
docker build . -f /home/ubuntu/tmp/docker-ide-ud-v3/Dockerfile -t tomtsang/ide-ok
```


## Ref {#ref}

-   <https://github.com/docker/for-mac/issues/1922>
