---
title: "Gamil Mail Server Domain"
date: 2018-08-09T15:44:37+08:00
draft: false
tags: ["mail"]
categories: ["mail"]
subtitle: "通过Gmail设置企业邮箱"
descriptions: ""
bigimg:
---

## Env

- 


## Step

因为我们 bitspace.link 的域名，注册国家为香港，因此您无法使用中国电话进行验证。

```
owen 您好， 

感谢您接听我的电话。很高兴与您取得联系。 

我了解您无法完成手机验证。由于您注册国家为香港，因此您无法使用中国电话进行验证。 

经调查，您可等待九天让系统自动删除帐户后再重新注册。或者您也可以通过以下所提供的CNAME纪录值来验证您的网域拥有权以让我协助您删除帐户： 

将以下的CNAME 纪录值根据主机及指向所需的DNS纪录值添加至您的网域管理控制台： 
-主机: deleteGAPPSnotBefore20180809utc 
-目标/指向: case-16595399-for-bitspace.link-at.google.com 
-Time to live (TTL): 3600 

如果您的网域代管商并非您的网域注册商，请确保您是透过网域代管商修改您的CNAME 记录值。更多关于建立CNAME记录值的步骤与相关资讯，请点阅支援中心文章： https://support.google.com/a/answer/47283?hl=zh-Hant 。若您在建立CNAME 记录值时需要协助，请联系您的网域代管商进一步协助进行操作。 

一旦完成了以上的验证，请回信告知以便我可以验证您对于“bitspace.link” 的拥有权。 

同时请您提供您无法使用电话号码验证的错误信息截图，之后我将协助进行删除帐户。之后我将协助进行删除帐户。 

若您还有其他疑问，您可以回覆此邮件以让我知道。我很乐意继续为您提供协助。
```

与google沟通，需要在Domain Name中配置

配置如下：

![domain-name-cname-configure](https://res.cloudinary.com/dmtixvmgt/image/upload/v1533806320/gmail.domain.owner_ejqvaj.png)

## Ref

- 