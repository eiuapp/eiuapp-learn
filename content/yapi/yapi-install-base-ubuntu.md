---
title: "Yapi Install Base Ubuntu"
date: 2018-06-28T18:37:43+08:00
draft: false
tags: ["yapi"]
categories: ["yapi"]
subtitle: "yapi安装"
descriptions: ""
bigimg:
---

## Env

- 


## Step

```
  177  yum install -y mongodb-org
  178  mongo -v
  179  systemctl start mongod
  180  /etc/init.d/mongod start
  181  npm install -g yapi-cli --registry https://registry.npm.taobao.org
  182  yapi server
  183  ls
  184  rm -rf my-yapi/
  185  clear
  186  ls
  187  node -v
  188  mongo
  189  git
  190  yum install git
  191  /etc/init.d/mongod restart
  192  mongod -v
  193  mongo
  194  clear
  195  npm install -g yapi-cli --registry https://registry.npm.taobao.org
  196  yapi server
  197  ls
  198  node my-yapi/vendors/server/app.js
  199  nohup node my-yapi/vendors/server/app.js &
  200  node my-yapi/vendors/server/app.js
  201  clear
  202  ps -ef | grep node
  203  kill -9 10314
  204  ps -ef | grep node
  205  ls
  206  cd my-yapi/
  207  ls
  208  cd vendors/
  209  ls
  210  cd ..
  211  node vendors/server/app.js
  212  ls
  213  cd my-yapi/
  214  ls
  215  cd vendors/
  216  ls
  217  cd ..
  218  clear
  219  ls
  220  cd ..
  221  netstat-ntpl
  222  netstat -ntpl
  223  ps -ef | grep node
  224  kill -9 1314
  225  ls
  226  cd my-yapi/
  227  ls
  228  cd vendors/
  229  ls
  230  cd my-yapi/
  231  ls
  232  cd vendors/
  233  ls
  234  node server/app.js
  235  ls
  236  ps -ef | grep node
  237  ls
  238  cd ..
  239  ls
  240  nohup node vendors/server/app.js &
  241  clear
  242  ifconfig
  243  history
  244  clear
  245  ls
  778  nohup node /my-yapi/vendors/server/app.js &
  779  history | grep nohup
  780  ps -ef | grep app.js
  781  ls
  782  nohup node my-yapi/vendors/server/app.js &
  783  exit
  784  netstat -ntpl

```


## Ref

- 
