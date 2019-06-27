---
title: "Ubuntu Install Wubipinyin"
date: 2018-05-24T09:32:16+08:00
draft: false
tags: ["ubuntu","install","wubipinyin"]
categories: ["install"]
subtitle: ""
descriptions: "ubuntu desktop 16.04 安装 五笔拼音输入法"
bigimg:
---



ubuntu安装 五笔拼音输入法

## env

- Ubuntu Desktop 16.04

## step 

#### 方法1

https://www.linuxdashen.com/ubuntu-16-04-%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85fcitx%E4%BA%94%E7%AC%94%E6%8B%BC%E9%9F%B3%E8%BE%93%E5%85%A5%E6%B3%95

#### 方法2

```
sudo add-apt-repository ppa:fcitx-team/nightly
sudo apt update -y
sudo apt -y install fcitx  
sudo apt -y install fcitx-table-wbpy
```

System Setting/Language Support/Keyboard input method system/ => fcitx

打开System Setting系统设置>Text Entry


![Text Entry](https://res.cloudinary.com/dmtixvmgt/image/upload/v1526977506/ubuntu-install-wbpy-step-1_rst6ad.png)

![选择+号](https://res.cloudinary.com/dmtixvmgt/image/upload/v1526977506/ubuntu-install-wbpy-step-2_ytokfn.png)

![搜索WubiPinyin并添加](https://res.cloudinary.com/dmtixvmgt/image/upload/v1526977506/ubuntu-install-wbpy-step-3_czfggq.png)

记得重启哟！


## ref

- https://www.linuxdashen.com/ubuntu-16-04-%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85fcitx%E4%BA%94%E7%AC%94%E6%8B%BC%E9%9F%B3%E8%BE%93%E5%85%A5%E6%B3%95
- https://blog.csdn.net/zzqlivecn/article/details/25018203
- https://blog.csdn.net/hamigua0208/article/details/51421117
- https://blog.csdn.net/e421083458/article/details/37738805
