---
title: "Hexo Command"
date: 2018-03-02T11:45:27+08:00
draft: false
tags: ["hexo", "command"]
categories: ["hexo"]
subtitle: ""
descriptions: ""
bigimg:
---

hexo系列之使用1

## hexo下新建页面下如何放多个文章？

https://www.zhihu.com/question/33324071



## hexo分类与tags配置

Hexo中如何给一篇文章加多个tags？
下面2种方式：

  * tags: [a,b,c] # 冒号后有一个空格
  * tags:
      `- tag1`
      `- tag2`


## 可以做到自动增加哟。

    tom@adata:~/m6s/blogs/tomtsang$ hexo n "mysql-21"
    INFO  Created: ~/m6s/blogs/tomtsang/source/_posts/mysql-21.md
    tom@adata:~/m6s/blogs/tomtsang$ hexo n "mysql"
    INFO  Created: ~/m6s/blogs/tomtsang/source/_posts/mysql-22.md
    tom@adata:~/m6s/blogs/tomtsang$ hexo n "mysql"
    INFO  Created: ~/m6s/blogs/tomtsang/source/_posts/mysql-23.md_

这样的话，方便形成系列的文章。每次就不用管是第几个mysql系列文章了。
不过系列文章，最好用的，还是 categories

## 可以用自定义的post:

    tom@adata:~/m6s/blogs/tomtsang$ hexo n "mysql" [mysql]
    INFO  Created: ~/m6s/blogs/tomtsang/source/_posts/mysql-4.md
    tom@adata:~/m6s/blogs/tomtsang$ hexo n "mysql" [mysql]
    INFO  Created: ~/m6s/blogs/tomtsang/source/_posts/mysql-5.md
    tom@adata:~/m6s/blogs/tomtsang$ hexo n "mysql" [mysql]
    INFO  Created: ~/m6s/blogs/tomtsang/source/_posts/mysql-1.md_

## end
