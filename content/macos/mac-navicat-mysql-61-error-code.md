

mac 本机安装 mysql8.0 后，navicat 连接 mysql 后报 61 错误码。怎么办？

## env

mac下mysql无法开启或者关闭

## step

在terminal中查看mysql的所有进程：ps aux |grep mysq*

查找到进程编号，使用kill ****关闭进程，如果没有权限在前面添加sudo，完成！


第一步：

点击系统偏好设置->最下边点MySQL，在弹出页面中，关闭服务



第二步：

进入终端输入：cd /usr/local/mysql/bin/
回车后 登录管理员权限 sudo su
回车后输入以下命令来禁止mysql验证功能 ./mysqld_safe --skip-grant-tables &
回车后mysql会自动重启（偏好设置中mysql的状态会变成running）

第三步：

输入命令 ./mysql
回车后，输入命令 FLUSH PRIVILEGES;
回车后，输入命令 SET PASSWORD FOR 'root'@'localhost' = PASSWORD('你的新密码');




## ref

- https://blog.csdn.net/a787188834/article/details/76152735
- https://blog.csdn.net/wujunwen/article/details/52039697
