

今天在服务器上配置了自己电脑的public key，但是还是不能直接免密码登录，

原因是

ssh 对目录的权限有要求，代码中要设置下新生成的config文件权限才行。

~目录权限是750，
~/.ssh 的是700， 
~/.ssh/* 的是600，
~/.ssh/config 是700


```
-rw-rw-r-- 1 webuser webuser  396 Feb 19 04:20 authorized_keys
$chmod 600 .ssh/authorized_keys
-rw------- 1 webuser webuser  396 Feb 19 04:20 authorized_keys
```

还有一次是，我死活登录不上 ubuntu@tencent-cloud 。
最后通过web连接登录，然后，机器上用ubuntu用户`ssh localhost`都登录不了，那就说明一定是权限问题了。
一看，原来，整个文件夹/home/ubuntu/ 的权限被更改成 tom:tom 了。那当然进不来了。所以这时：
```shell
sudo chown ubuntu:ubuntu /home/ubuntu/
```
这样，才能ssh 登录正常。

## ref:

- https://blog.csdn.net/shiralwz/article/details/50697207  

----------------------


假如在生成密钥对的时候指定了其他文件名（或者需要控制N台机器，此时你会生成多对密钥），则需要使用参数-i指定私钥文件

```shell
$ ssh ubuntu@192.68.0.11 -i ./id_rsa
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
New release '18.04.1 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


Last login: Thu Jan 17 09:42:08 2019 from 113.91.43.118
ubuntu@VM-0-12-ubuntu:~$ 
```

放在前面也是可以的

```shell
$ ssh -i ./id_rsa ubuntu@111.230.153.251 
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
New release '18.04.1 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


Last login: Thu Jan 17 09:52:45 2019 from 113.91.43.118
ubuntu@VM-0-12-ubuntu:~$ 
```

## ref
- http://www.cnblogs.com/ma6174/archive/2012/05/26/2519458.html
