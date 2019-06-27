---
title: "量化小学Lecture4的笔记"
date: 2018-10-17T09:18:35+08:00
draft: false
tags: ["jaqs", "python"]
categories: ["python"]
subtitle: "matplotlib.finance已经废弃了，怎么办？"
descriptions: ""
bigimg:
---

## Env

- os: mac


## Step

尝试用python做个股票绘图软件，要用到 finance 库，于是开始导入：

```
import matplotlib.finance as mpf
```
结果执行的时候直接报错：

```
ModuleNotFoundError: No module named 'matplotlib.finance'
```

那么我们可以到 ipython 环境中去测试一下。

```
➜  ~ ipython
Python 3.7.0 (default, Jun 28 2018, 07:39:16)
Type 'copyright', 'credits' or 'license' for more information
IPython 6.5.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: from matplotlib.finance import candlestick_ohlc
---------------------------------------------------------------------------
ModuleNotFoundError                       Traceback (most recent call last)
<ipython-input-1-7ea83a59eaf3> in <module>()
----> 1 from matplotlib.finance import candlestick_ohlc

ModuleNotFoundError: No module named 'matplotlib.finance'
```

去查看 matplotlib 的文档说明，在matplotlib2.2.2的API中有这么一段话：

```
The matplotlib.finance, mpl_toolkits.exceltools and mpl_toolkits.gtktools modules have been removed. matplotlib.finance remains available at https://github.com/matplotlib/mpl_finance.
```

finance这个模块竟然被删除了！！！并且就是从2.2.2版本开始。

知道了原因，解决方法就简单了，在[github](https://github.com/matplotlib/mpl_finance)中下载源代码，安装：

```
git clone https://github.com/matplotlib/mpl_finance && cd mpl_finance
python setup.py install
```
可以看到 mpl_finance 模块已经安装上了。

### 验证

```
➜  mpl_finance git:(master) ✗ ipython
Python 3.7.0 (default, Jun 28 2018, 07:39:16)
Type 'copyright', 'credits' or 'license' for more information
IPython 6.5.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: from matplotlib.mpl_finance import candlestick_ohlc
---------------------------------------------------------------------------
ModuleNotFoundError                       Traceback (most recent call last)
<ipython-input-1-89e3069fc596> in <module>()
----> 1 from matplotlib.mpl_finance import candlestick_ohlc

ModuleNotFoundError: No module named 'matplotlib.mpl_finance'

In [2]: from mpl_finance import candlestick_ohlc

In [3]: exit
➜  mpl_finance git:(master) ✗
```

## Ref

- http://www.classnotes.cn/3689.html
- https://github.com/matplotlib/mpl_finance