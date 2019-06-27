---
title: "Php Install Base Ubuntu"
date: 2018-06-16T18:11:25+08:00
draft: false
tags: ["ubuntu", "php"]
categories: ["php"]
subtitle: "ubuntu16.04如何安装php5"
descriptions: ""
bigimg:
---

## Env

- os: ubuntu

## Step

```
sudo apt-get install -y language-pack-en-base
sudo LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt search php5   # 这个地方，会返回具体的版本号，不同的时间可能返回不同，比如20180616返回的是5.6，所以安装php5.6而不是php5.5
sudo apt-get install php5.6-common
sudo apt-get install libapache2-mod-php5.6
```

## Ref

- https://www.zhihu.com/question/45999546/answer/100165171
