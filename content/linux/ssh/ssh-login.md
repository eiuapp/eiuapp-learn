---
title: "Ssh Login"
date: 2018-05-24T15:22:36+08:00
draft: false
tags: ["ssh"]
categories: ["ssh"]
subtitle: ""
descriptions: ""
bigimg:
---


## faq

报错：Too many authentication failures

```
tom@tom-w10vbud16:~/ztom/bits/pem$ ssh -i vachain_competition.pem ubuntu@ec2-13-125-197-97.ap-northeast-2.compute.amazonaws.com
Received disconnect from 13.125.197.97 port 22:2: Too many authentication failures
Connection to ec2-13-125-197-97.ap-northeast-2.compute.amazonaws.com closed by remote host.
Connection to ec2-13-125-197-97.ap-northeast-2.compute.amazonaws.com closed.
tom@tom-w10vbud16:~/ztom/bits/pem$ 
```

If you are not using any ssh hosts configuration, you have to explicitly specify the correct key in the ssh command like so:
```
ssh -i some_id_rsa -o 'IdentitiesOnly yes' them@there:/path/
```
Note: the 'IdentitiesOnly yes' parameter needed to be between quotes.

or
```
ssh -i some_id_rsa -o IdentitiesOnly=yes them@there:/path/
```

那好吧，我们试一下，果然登录成功了

```
tom@tom-w10vbud16:~/ztom/bits/pem$ ssh -i vachain_competition.pem  -o 'IdentitiesOnly yes'  ubuntu@ec2-13-125-197-97.ap-northeast-2.compute.amazonaws.com
Welcome to Ubuntu 16.04.4 LTS (GNU/Linux 4.4.0-1052-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


*** System restart required ***
Last login: Thu May 24 07:18:32 2018 from 121.34.145.43
ubuntu@ip-172-31-5-249:~$ 
```

## Ref

- https://www.cnblogs.com/xueweihan/p/6346804.html