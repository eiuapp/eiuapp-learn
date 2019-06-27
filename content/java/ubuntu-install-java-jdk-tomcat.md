# Ubuntu18.04配置Java环境和安装Tomcat的方法（详细）

https://blog.csdn.net/qq_31954797/article/details/80149504

## env

- os: ubuntu 18.04
- jdk: jdk1.8.0_72
- tomcat: apache-tomcat-9.0.20

## 两个安装包：JavaJDK，Tomcat

如果说真的是去下载，需要帐户，有了帐户，不一定登录得上。尴尬。

- 搜索 oracle 帐户

https://blog.csdn.net/CSDNno/article/details/79397566

下载去吧

- 搜索 jdk 

https://wiki.jikexueyuan.com/project/linux-in-eye-of-java/JDK-Install.html 提供了一个百度云下载地址

tomcat 地址，wget http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-9/v9.0.7/bin/apache-tomcat-9.0.7.tar.gz ，

也是已经失效了，但是，思路是在的。打开 http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-9/ ，有 v9.0.19 那就直接把上面的地址改成：

http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-9/v9.0.19/bin/apache-tomcat-9.0.19.tar.gz 就可以了。或者按这个地址，找一下，就行了。

## 配置Javajdk

```bash
ubuntu@utuntu:~/lcnx/aliyun/env$ cd /home/ubuntu/lcnx/aliyun/env/
ubuntu@utuntu:~/lcnx/aliyun/env$ tar -zxvf ./jdk-8u72-linux-x64.tar.gz
ubuntu@utuntu:~/lcnx/aliyun/env$ ln -s ./jdk1.8.0_72/ java
ubuntu@utuntu:~/lcnx/aliyun/env$ ls /home/ubuntu/lcnx/aliyun/env/java
bin  COPYRIGHT  db  include  javafx-src.zip  jre  lib  LICENSE  man  README.html  release  src.zip  THIRDPARTYLICENSEREADME-JAVAFX.txt  THIRDPARTYLICENSEREADME.txt
ubuntu@utuntu:~/lcnx/aliyun/env$ sudo vi /etc/profile
ubuntu@utuntu:~/lcnx/aliyun/env$ tail -10 /etc/profile
LCNX_HOME=/home/ubuntu/lcnx/aliyun/env/
JAVA_HOME=$LCNX_HOME/java
JRE_HOME=$JAVA_HOME/jre
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME
export JRE_HOME
export PATH
export CLASSPATH
ubuntu@utuntu:~/lcnx/aliyun/env$ source /etc/profile
ubuntu@utuntu:~/lcnx/aliyun/env$ echo $JAVA_HOME
/home/ubuntu/lcnx/aliyun/env/java
ubuntu@utuntu:~/lcnx/aliyun/env$ java -version
java version "1.8.0_72"
Java(TM) SE Runtime Environment (build 1.8.0_72-b15)
Java HotSpot(TM) 64-Bit Server VM (build 25.72-b15, mixed mode)
ubuntu@utuntu:~/lcnx/aliyun/env$ 
```

## 配置Tomcat

```bash
ln -s ./tomcat
ubuntu@utuntu:~/lcnx/aliyun/env$ vi tomcat/
bin/             BUILDING.txt     conf/            CONTRIBUTING.md  lib/             LICENSE          logs/            NOTICE           README.md        RELEASE-NOTES    RUNNING.txt      temp/            webapps/         work/            
ubuntu@utuntu:~/lcnx/aliyun/env$ vi tomcat/bin/startup.sh 
ubuntu@utuntu:~/lcnx/aliyun/env$ tail -14 tomcat/bin/startup.sh 
# set java environment
LCNX_HOME=/home/ubuntu/lcnx/aliyun/env
JAVA_HOME=$LCNX_HOME/java
JRE_HOME=$JAVA_HOME/jre
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME
export JRE_HOME
export PATH
export CLASSPATH

# tomcat
export TOMCAT_HOME=$LCNX_HOME/tomcat

ubuntu@utuntu:~/lcnx/aliyun/env$ 
```

## 启动Tomcat

```bash
ubuntu@utuntu:~/lcnx/aliyun/env$ ln -s ./apache-tomcat-9.0.20/ tomcat
ubuntu@utuntu:~/lcnx/aliyun/env$ cd tomcat/bin/
ubuntu@utuntu:~/lcnx/aliyun/env/tomcat/bin$ ./startup.sh 
Using CATALINA_BASE:   /home/ubuntu/lcnx/aliyun/env/tomcat
Using CATALINA_HOME:   /home/ubuntu/lcnx/aliyun/env/tomcat
Using CATALINA_TMPDIR: /home/ubuntu/lcnx/aliyun/env/tomcat/temp
Using JRE_HOME:        /home/ubuntu/lcnx/aliyun/env/java/jre
Using CLASSPATH:       /home/ubuntu/lcnx/aliyun/env/tomcat/bin/bootstrap.jar:/home/ubuntu/lcnx/aliyun/env/tomcat/bin/tomcat-juli.jar
Tomcat started.
ubuntu@utuntu:~/lcnx/aliyun/env/tomcat/bin$ netstat -tnlp | grep 8080
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp6       0      0 :::8080                 :::*                    LISTEN      21554/java          
ubuntu@utuntu:~/lcnx/aliyun/env/tomcat/bin$ 
```

通过chrome也是可以访问的。