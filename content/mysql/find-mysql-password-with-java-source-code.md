---
title: "java源码找到mysql用户密码"
date: 2018-03-02T12:15:19+08:00
draft: false
tags: ["mysql", "mariadb", "cluster"]
categories: ["mysql"]
subtitle: ""
descriptions: ""
bigimg:
---

在有java源码的情况下，找到mysql用户密码

## 进入项目查找 mysql ip 字符串

```
lcnx@iZwz95dxhc92qtibd4f399Z:~/server/tomcat-data$ grep "aliyuncs" -rn ./bin/
lcnx@iZwz95dxhc92qtibd4f399Z:~/server/tomcat-data$ grep "aliyuncs" -rn ./conf/
lcnx@iZwz95dxhc92qtibd4f399Z:~/server/tomcat-data$ grep "aliyuncs" -rn ./lib/
lcnx@iZwz95dxhc92qtibd4f399Z:~/server/tomcat-data$ grep "aliyuncs" -rn ./webapps/
./webapps/lcnxdata/WEB-INF/classes/applicationContext.xml:30:        <value>jdbc:mysql://rm-wz95k1f761xu890f3.mysql.rds.aliyuncs.com:3306/lcnx</value>
lcnx@iZwz95dxhc92qtibd4f399Z:~/server/tomcat-data$ cat ./webapps/lcnxdata/WEB-INF/classes/applicationContext.xml 

<bean id="dataSource" class="org.logicalcobwebs.proxool.ProxoolDataSource">
    <property name="driver">
        <value>com.mysql.jdbc.Driver</value>
    </property>
    <property name="driverUrl">
        <!-- <value>jdbc:mysql://192.168.0.188:3306/nemmp</value> -->
        <value>jdbc:mysql://rm-wz95k1f761xu890f3.mysql.rds.aliyuncs.com:3306/lcnx</value>
    </property>
    <property name="user" value="*********" />
    <property name="password" value="**********" />
    <property name="alias" value="*********" />
   
    <property name="prototypeCount" value="150" />
    <property name="maximumConnectionCount" value="1100" />
    <property name="minimumConnectionCount" value="300" />
    <property name="simultaneousBuildThrottle" value="300" />
    <property name="houseKeepingTestSql" value="select CURRENT_DATE" />
    <property name="trace" value="true" />
</bean>

lcnx@iZwz95dxhc92qtibd4f399Z:~/server/tomcat-data$ 
```

确认mysql可通后，用帐号密码连接就可以了。

```
lcnx@iZwz95dxhc92qtibd4f399Z:~/server/tomcat-data$ telnet rm-wz95k1f761xu890f3.mysql.rds.aliyuncs.com 3306
Trying 172.18.70.94...
Connected to rm-wz95k1f761xu890f3.mysql.rds.aliyuncs.com.
Escape character is '^]'.
J
5.6.70f𞒭UfJG%D࠷!oD&:,@tTO)G.Amysql_native_password
^C
Connection closed by foreign host.
lcnx@iZwz95dxhc92qtibd4f399Z:~/server/tomcat-data$ which mysql
/alidata/server/mysql/bin/mysql
lcnx@iZwz95dxhc92qtibd4f399Z:~/server/tomcat-data$ mysql -h rm-wz95k1f761xu890f3.mysql.rds.aliyuncs.com -u user -p 
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 537495688
Server version: 5.6.70 Source distribution

Copyright (c) 2000, 2014, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
Bye
lcnx@iZwz95dxhc92qtibd4f399Z:~/server/tomcat-data$ 
```

