---
title: "Gitlab CI Step by Step"
date: 2018-07-10T22:26:33+08:00
draft: false
tags: ["gitlab"]
categories: ["gitlab"]
subtitle: "gitlab-ci入门"
descriptions: ""
bigimg:
---

gitlab-ci 一步一步学

## Env

- os: ubuntu
- docker: 17.03.2
- gitlab: 10.2.8(比特空间提供)

[^_^]:
    - ip: 120.77.45.61
- gitlab-runner: 

[^_^]:
    - ip: 13.209.19.195

## Step

### 理论

#### 了解

- [用 GitLab CI 进行持续集成](https://scarletsky.github.io/2016/07/29/use-gitlab-ci-for-continuous-integration/), 这篇文章是写得我认为最合适入门的。

- [gitlab ci quick-start](https://docs.gitlab.com/ce/ci/quick_start/README.html)有一个对应的[中文版本-GitLab CI/CD快速入门](http://www.ttlsa.com/auto/gitlab-cicd-quick-start/)。


### 安装 gitlab

略

### 安装 gitlab-runner

- 可以先直接看 [一步一步完成GitLab Runner持续化自动部署](https://blog.csdn.net/shuyuea3/article/details/80699073) 的前3大点，就是安装的过程，我也就是这么安装成功的。


从 https://docs.gitlab.com/runner/install/ 了解到 [docker 安装 gitlab-runner](https://docs.gitlab.com/runner/install/docker.html)

```
docker run -d --name gitlab-runner --restart always \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:v11.0.0
```

### registering-a-specific-runner

#### 获取Gitlab项目的Token

https://blog.csdn.net/u011215669/article/details/80458972

#### register
https://docs.gitlab.com/ce/ci/runners/README.html#registering-a-specific-runner

找到

https://docs.gitlab.com/runner/register/#docker

可以完成 register, 但是，真正操作，其实可以参考刚刚说过的前3点大点。

### 例子

https://gitlab.com/gitlab-examples/nodejs

这里有一个nodejs的例子，可以先看一下项目中的 `.gitlab-ci.yml` 差不多出就明白了。

那怎么利用这个例子呢？来吧。
总体思路，把这个工程clone下，push到自己的gitlab的一个新项目中去，然后，修改`.gitlab-ci.yml` 中的 tag为我们自己gitlab-runner的tag，然后commit就可以了。

具体命令如下。

在 gitlab-runner 节点

```
sudo docker run   --name postgres-db   --publish=5432:5432   -e POSTGRES_PASSWORD=123456   -e POSTGRES_USER=testuser   -d postgres:9.5.0 ## 因为这个项目的README.md文件中说明了，要先启动这个container,所以我们就在 gitlab-runner节点上启动一个。
git clone https://gitlab.com/gitlab-examples/nodejs.git
cd nodejs/
git remote add gitlab http://X.X.X.X/archive/nodejs-test-a.git
git push gitlab master
vi .gitlab-ci.yml  ### 修改tag为 `my-tag`
git status
git add .gitlab-ci.yml
git commit -m "fix .gitlab-ci.yml"
git push gitlab master
```

然后回到 gitlab，就可以看到CI/CD中的流水线中的作业，在执行我们想要的命令。

### 学习 `yaml` 文件

接下来，我们当然是要去学习`.gitlab-ci.yml`的写法了。
来看看 .gitlab-ci.yml 是怎么写的吧.

[官方文档](https://docs.gitlab.com/ce/ci/yaml/README.html)

对应地，中文翻译

- 有一个过时的[中文翻译](https://fennay.github.io/gitlab-ci-cn/gitlab-ci-yaml.html)
- 过时翻译，加了一个[目录](https://www.ctolib.com/topics-121471.html)
- 过时翻译，加了一个[目录](https://segmentfault.com/a/1190000010442764)

另一个翻译

- [翻译](https://blog.csdn.net/wmq880204/article/details/70141771)

[更多官方文档中文翻译](https://fennay.github.io/gitlab-ci-cn/)


## Ref

- https://scarletsky.github.io/2016/07/29/use-gitlab-ci-for-continuous-integration/
- https://docs.gitlab.com/ce/ci/yaml/README.html
- https://docs.gitlab.com/ce/ci/runners/README.html#registering-a-specific-runner


