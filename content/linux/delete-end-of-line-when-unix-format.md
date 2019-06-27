

Linux下删除^M文件的方法
===============


Unix系统里，每行结尾只有“<换行>”，即“\\n”；Windows系统里面，每行结尾是“<换行><回车>”，即“\\n\\r”；Mac系统里，每行结尾是“<回车>”。

## 1\. 问题描述：

在windows下写的文件上传到Linux服务器之后,文件中多出了很多^M符号

## 2\. 原因分析：

Linux和windows的文本中，对换行方式处理不同：

    '\n' 10 换行（newline）
    '\r' 13 回车（return）
    
| 系统   |     换行方式     | 
|----------|:-------------:|
| Windows | 结尾是<换行><回车>,即“\\n\\r”| 
|linux/unix |  结尾是<换行>,即 “\\n” | 
| Mac      |     结尾是<回车>,即“\\r”   |      
 
所以windows下的文件，在Linux中会有^M，即回车符号  
参考：[回车符和换行符的区别](https://link.jianshu.com?t=http://dadoneo.iteye.com/blog/984725)

## 3.解决办法：

解决办法主要以下几个方案：  
_注意： ^M要用Ctrl+v，<回车>代替_


| 命令   |      
|:-------------|
|1\. vim 中使用替换命令：`:%s/^M//g` | 
|2\. 使用sed:`sed 's/^M//' filename > newfile` | 
|3\. 使用tr删除“\\r”：`tr -d "\r" filename` | 
|4\. 使用dos2unix命令：`dos2unix filename` | 
|5\. 在vim下：`:set ff = unix`（把dos文件类型变为unix）| 


此外，也可以使用sed把win文档转化为Linux下文档:  
`find . -type f print0 | xargs -0 sed -i 's/^M$//'`  
其中实践中试验了第一种方法，举例说明该命令的含义：  
将文件中的 a 全部替换为b,可以使用`:%s/a/b/g`  


有时候，你会发现，当文件已经是 unix 格式时，依然会在行尾有 `^M`，这时，通过 以上几种方式依然不能删除这个`^M`，那么请使用：

`sed -i 's#^M##g' filename`

```
DELL@DESKTOP-MQ9CENU MINGW64 /f/org-notes (master)
$ sed -i 's#^M##g' ./gtd.org

DELL@DESKTOP-MQ9CENU MINGW64 /f/org-notes (master)
$ vi gtd.org
```

## 参考  
- [去掉Linux中删除^M符号的方法](https://link.jianshu.com?t=http://mrcelite.blog.51cto.com/2977858/745576)  
- [vim如何去掉^M字符](https://link.jianshu.com?t=https://www.zhihu.com/question/22130727)
- https://www.zhihu.com/question/22130727

