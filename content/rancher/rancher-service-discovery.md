---
title: "Rancher Service Discovery"
date: 2018-06-30T09:06:04+08:00
draft: false
tags: ["rancher"]
categories: ["rancher"]
subtitle: "rancher 中的 service discovery"
descriptions: ""
bigimg:
---

rancher 中的 service discovery
## Env

- rancher: v2.0.2

## Step

service discovery对应于 k8s的service

- external IP addresses 相当于 DNS的A记录的解析
- An external hostname 相当于 DNS的CNAME的域名转换
- Alias of another DNS record's value 相当于 指向另外一个DNS的记录
- One or more workloads 指向其它的workloads
- The set of pods which match a selector 通过selector作转换, 实现内部的负载均衡，升级时就可使用。

## Ref

- http://live.vhall.com/209459630