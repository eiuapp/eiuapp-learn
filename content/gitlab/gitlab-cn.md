---
title: "Gitlab汉化"
date: 2018-04-17T09:50:03+08:00
draft: false
tags: ["gitlab", "git"]
categories: ["git"]
subtitle: ""
descriptions: ""
bigimg:
gitment: true
---

对gitlab进行汉化

## env

IP: 192.168.31.148
OS: ubuntu-14.04.5
gitlab: 10.6.4

## step

```
mkdir xhang && cd xhang
git clone https://gitlab.com/xhang/gitlab.git
cd gitlab/
git fetch
gitlab_version=$(sudo cat /opt/gitlab/embedded/service/gitlab-rails/VERSION)
echo ${gitlab_version}
git diff v${gitlab_version} v${gitlab_version}-zh > ../${gitlab_version}-zh.diff
cd ..
sudo gitlab-ctl stop
sudo patch -d /opt/gitlab/embedded/service/gitlab-rails -p1 < 10.6.4-zh.diff
sudo gitlab-ctl start
sudo gitlab-ctl reconfigure
telnet 192.168.31.148 7890
```

## ref

- [汉化指南，基于 Larry Li 版汉化指南 修改](https://gitlab.com/xhang/gitlab/wikis/home)