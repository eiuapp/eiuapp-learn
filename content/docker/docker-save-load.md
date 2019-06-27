# docker save and docker load

https://blog.csdn.net/weixin_36343850/article/details/80553680

## docker save

```bash
vagrant@ubuntu-xenial:~$ docker stop some-redis
some-redis
vagrant@ubuntu-xenial:~$ docker ps -a | grep some-redis
b3e246f35c82        redis:5.0.4                          "docker-entrypoint.s…"   2 weeks ago         Exited (0) 2 seconds ago                                                                  some-redis
ubuntu-xenial:~$ docker commit some-redis some-redis-save
sha256:69ee84b5567bf37f24b81669f7341b6c9730d3a9b78b95d908b22a2cdcc334bd
vagrant@ubuntu-xenial:~$ docker images | grep some-redis
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
some-redis-save              latest              69ee84b5567b        9 seconds ago       95MB
vagrant@ubuntu-xenial:~$ docker save -o some-redis-save.tar some-redis-save
```

## docker load

```bash
ubuntu@utuntu:~$ docker load -i ./some-redis-save.tar
5dacd731af1b: Loading layer [==================================================>]  58.45MB/58.45MB
b52995330c04: Loading layer [==================================================>]  338.4kB/338.4kB
1540700d9a4f: Loading layer [==================================================>]  3.034MB/3.034MB
f3918e45d85b: Loading layer [==================================================>]  36.43MB/36.43MB
290c0f1ac500: Loading layer [==================================================>]  1.536kB/1.536kB
f02e80b7746e: Loading layer [==================================================>]  3.584kB/3.584kB
Loaded image: some-redis-save:latest
ubuntu@utuntu:~$ ls
some-redis-save.tar
ubuntu@utuntu:~$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
some-redis-save     latest              69ee84b5567b        3 minutes ago       95MB
hello-world         latest              fce289e99eb9        4 months ago        1.84kB
```

但是这里的镜像，没有把层拿过来，导致，我`pull redis:5.0.4`依然要去 hub.docker.com 中拉取

```bash
ubuntu@utuntu:~$ docker pull redis:5.0.4
5.0.4: Pulling from library/redis
743f2d6c1f65: Pull complete 
171658c5966d: Pull complete 
fbef10bd7a65: Pull complete 
98afd60e45e4: Pull complete 
495c87fda859: Pull complete 
ed6767858416: Pull complete 
Digest: sha256:2dfa6432744659268d001d16c39f7be52ee73ef7e1001ff80643f0f7bdee117e
Status: Downloaded newer image for redis:5.0.4
```

## start

```bash
ubuntu@utuntu:~$ docker run --name some-redis -p 6379:6379  -v /home/ubuntu/docker/data/some-redis:/data -d some-redis-save redis-server --appendonly yes
d08919c3edeb529c8dc2f424ccba548652fcd84afa6422ff7fe3c505f60c5f0f
ubuntu@utuntu:~$ docker ps 
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
d08919c3edeb        some-redis-save     "docker-entrypoint.s…"   3 seconds ago       Up 1 second         0.0.0.0:6379->6379/tcp   some-redis
ubuntu@utuntu:~$ docker exec -it some-redis bash
root@d08919c3edeb:/data# ls -a
.  ..  appendonly.aof
root@d08919c3edeb:/data# redis-cli
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> exit
root@d08919c3edeb:/data# exit
exit
ubuntu@utuntu:~$ 
ubuntu@utuntu:~$ ls ~/docker/data/some-redis/ -a
.  ..  appendonly.aof
docker  some-redis-save.tar
ubuntu@utuntu:~$ telnet 127.0.0.1 6379
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
^]
telnet> quit
Connection closed.
ubuntu@utuntu:~$ 
```

## mysql 之 docker save, docker load ##

### mysql 之 docker save, docker load。 ###

发现 **mysql 本身的数据，没有随 docker save image 迁移**

那么这时，要完成mysql数据的迁移，就要完成下面几步：

- docker 中的数据，则只能 mysqldump 成 sql文件,  `docker exec -it mysql03 mysqldump -uroot -p123456 lcnx_20190512 > /home/vagrant/mysql03-lcnx_20190512.sql`
- 然后，scp到新机器，`scp mysql03-lcnx_20190512.sql ubuntu@lanServer:/home/ubuntu/docker/container/mysql03/`
- 然后，新机器, create database, `CREATE DATABASE IF NOT EXISTS lcnx_20190512 CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;`
- 然后，进入 docker container, `docker exec -it mysql03-20190525 bash`
- 进入 mysql, ` mysql -uroot -p`
- 然后source *.sql文件. `source /root/mysql03-lcnx_20190512.sql;` 如果报错，看下面的内容。

https://gist.github.com/spalladino/6d981f7b33f6e0afe6bb
https://blog.csdn.net/qq_37674858/article/details/80082903

```bash
# Backup
docker exec CONTAINER /usr/bin/mysqldump -u root --password=root DATABASE > backup.sql

# Restore
cat backup.sql | docker exec -i CONTAINER /usr/bin/mysql -u root --password=root DATABASE
```

试问，有没有更好的在docker间迁移mysql的方式？

### mysql 之 docker volume ###

https://www.cnblogs.com/JacZhu/p/7835237.html

```bash
利用 Docker 备份、迁移数据库

最近在把腾讯云的国内主机迁移到香港主机，因为之前使用的 MySql 跟 MongoDb 都是基于 Docker 部署的，所以迁移起来还算比较方便，主要思路就是把数据库容器的数据卷单独做成一个数据镜像，然后把这个镜像提交到香港主机上面的私有仓库，最后用这个镜像生成一个数据容器挂载到应用容器上就好了。

1. 备份数据卷
docker run --rm --volumes-from data-container-backup --name tmp-backup -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /folderToBackup
    
#Example: Backup mysql database
docker run --rm --volumes-from blog-mysql --name tmp-backup -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /var/lib/mysql
--rm 用来创建一个“用完即销”的容器，--volumes-from 用来把一个已有容器上挂载的卷挂载到新创建的容器上

2. 创建数据容器
docker run -d -v $(pwd):/backup --name data-backup alpine /bin/sh -c "cd / && tar xvf /backup/backup.tar"
3. 推送数据容器到私有仓库
docker commit data-backup registry-host:port/data-backup:$VERSION

docker push registry-host:port/data-backup:$VERSION
4. 在另一台主机下载数据容器
docker run -v /folderToBackup --entrypoint "bin/sh" --name data-container registry-host:port/data-backup:${VERSION}
5. 将数据容器里面的数据卷挂载到应用容器上
docker run --volumes-from=data-container registry-host:port/data-backup:${VERSION}

# Example
docker run --name new-mysql -d -p 3306:3306 --volumes-from=data-container registry-host:port/data-backup:${VERSION}
就这样 5 步操作，就可以很方便的备份、迁移数据库了。所以买主机也一定要买支持 Docker 的 KVM 虚拟机啊。
```

这种方式，还没有尝试，等会要试一下。

## mysql error ##

### MySQL ERROR 1231 (42000):Variable 'character_set_client' can't be set to the value of 'NULL' ###

mysql 导入数据 source x.sql 时，报这个错误。

https://stackoverflow.com/questions/29112716/mysql-error-1231-42000variable-character-set-client-cant-be-set-to-the-val

Added the following text at the beginning of the mysqldump file and the restore was successful.

```bash
/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE='+00:00' */;
/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
```

还有一种 次优解

https://blog.csdn.net/lipeigang1109/article/details/78810679
https://dba.stackexchange.com/questions/153510/error-on-re-import-1231-variable-character-set-client-cant-be-set-to-the

