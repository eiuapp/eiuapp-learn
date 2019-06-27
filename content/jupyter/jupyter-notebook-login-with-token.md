+++
title = "jupyter-notebook 密码查询"
date = 2018-12-21T00:00:00+08:00
lastmod = 2018-12-21T09:53:40+08:00
tags = ["python", "notebook"]
categories = ["python"]
draft = false
weight = 3006
+++

有一些场景，需要连接jupyter-notebook, 这时，需要提供登陆密码.

这个密码就是 token.

```shell
(py3) ➜  learn-python3 git:(master) ✗  /Users/tomtsang/Downloads/.ana/bin/jupyter-notebook list
Currently running servers:
http://localhost:8888/?token=b488be28e8f4bdf6f11b3e863ee92896d99c50ed70e67bde :: /Users/tomtsang
(py3) ➜  learn-python3 git:(master) ✗
```

如果是安装的 anaconda ，则在 anaconda 启动 jupyter-notebook 的过程中会提示出这个
token. 如下：

```shell
➜  ~ git:(master) ✗ /Users/tomtsang/Downloads/.ana/bin/jupyter_mac.command ; exit;
[I 09:36:46.923 NotebookApp] JupyterLab extension loaded from /Users/tomtsang/Downloads/.ana/lib/python3.7/site-packages/jupyterlab
[I 09:36:46.924 NotebookApp] JupyterLab application directory is /Users/tomtsang/Downloads/.ana/share/jupyter/lab
[I 09:36:46.931 NotebookApp] Serving notebooks from local directory: /Users/tomtsang
[I 09:36:46.931 NotebookApp] The Jupyter Notebook is running at:
[I 09:36:46.931 NotebookApp] http://localhost:8888/?token=b488be28e8f4bdf6f11b3e863ee92896d99c50ed70e67bde
[I 09:36:46.931 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 09:36:46.936 NotebookApp]

    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8888/?token=b488be28e8f4bdf6f11b3e863ee92896d99c50ed70e67bde
[I 09:36:47.210 NotebookApp] Accepting one-time-token-authenticated connection from ::1

```

第6行中的
\`<http://localhost:8888/?token=b488be28e8f4bdf6f11b3e863ee92896d99c50ed70e67bde>\`
就是地址和token了。


## Ref {#ref}

-   <https://blog.csdn.net/e35t66/article/details/76274714>
