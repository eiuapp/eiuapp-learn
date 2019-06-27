+++
title = "Shell(Bash)中如何判断是否存在某个命令"
date = 2018-12-16T00:00:00+08:00
lastmod = 2018-12-16T12:08:10+08:00
tags = ["shell", "check"]
categories = ["shell"]
draft = false
weight = 3001
+++

在编写bash时，如果要判断某条命令是否存在，应该如何写呢？
下面以 foo 代表某个命令（如：wget）.
我尝试了如下的写法，

```
which foo > /dev/null 2>&1
if [ $? == 0 ]; then
    echo "exist"
else
    echo "dose not exist"
fi
```

但是：
最好避免使用 which，做为一个外部的工具，并不一定存在，在发行版之间也会有区别，有的系统的 which 命令不会设置有效的 exit status，存在一定的不确定性。

Bash 有提供一些内建命令如 hash、type、command 也能达到要求。

```
$ command -v foo >/dev/null 2>&1 || { echo >&2 "I require foo but it's not installed.  Aborting."; exit 1; }
$ type foo >/dev/null 2>&1 || { echo >&2 "I require foo but it's not installed.  Aborting."; exit 1; }
$ hash foo 2>/dev/null || { echo >&2 "I require foo but it's not installed.  Aborting."; exit 1; }
```

详见 <https://stackoverflow.com/questions/592620/how-to-check-if-a-program-exists-from-a-bash-script>
