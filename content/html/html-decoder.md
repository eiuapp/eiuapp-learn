---
title: "Html Decoder"
date: 2018-03-08T15:51:23+08:00
draft: false
tags: ["html", "decoder"]
categories: ["html"]
subtitle: ""
descriptions: ""
bigimg:
---



## 在线解码

http://www.ofmonkey.com/encode/unicode

## html-decoder 包的使用

#### install

```
[jlch@check gen]$ sudo npm install -g html-decoder
[jlch@check gen]$ cd /usr/local/lib/node_modules/html-decoder/ 
[jlch@check html-decoder]$ ls
bin  data  dist  Gruntfile.js  LICENSE  package.json  README.md  src  tests
```

#### 测试

```
[jlch@check html-decoder]$ sudo vi  data/entities.json ## 把想decoder的内容放进去
[jlch@check html-decoder]$ sudo ./bin/genhtmlentities  data/entities.json
Completed in 49 milliseconds!
[jlch@check html-decoder]$ cat ./src/gen/trie.json ## 这个是存放结果的地方
{"1":{},"2":{},"3":{},"5":{},"6":{},"7":{},"8":{},"9":{}}
[jlch@check html-decoder]$ ll ./src/gen/trie.json
-rw-r--r-- 1 root root 57 3月   8 15:49 ./src/gen/trie.json
[jlch@check html-decoder]$
```



