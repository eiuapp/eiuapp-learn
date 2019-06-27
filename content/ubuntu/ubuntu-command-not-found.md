---
title: "Ubuntu Command Not Found"
date: 2018-06-16T18:08:48+08:00
draft: false
tags: ["ubuntu"]
categories: ["ubuntu"]
subtitle: "ubuntu command not found"
descriptions: ""
bigimg:
---

## add-apt-repository: command not found

When I run:
```
sudo add-apt-repository ppa:ubuntu-wine/ppa
```
Log here:
```
sudo: add-apt-repository: command not found
```

### solved

I tried to run:
```
sudo apt-get install software-properties-common
```

