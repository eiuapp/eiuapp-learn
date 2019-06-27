

vscode integrated terminal and git

## env

- os: win10
- vscode: 1.30.2

## step

我的git安装在 “D:\\Program Files\\Git\\” 下， 最后的配置结果如下：

```
{
    "git.autofetch": true,
    "git.confirmSync": false,
    "terminal.external.windowsExec": "D:\\Program Files\\Git\\git-bash.exe",
    "terminal.integrated.shell.windows": "D:\\Program Files\\Git\\bin\\bash.exe",
    "git.path": "D:\\Program Files\\Git\\bin\\git.exe",
    "files.eol": "\n"
}
```

### 如果报错 “no source control providers registered”

如果报错 “no source control providers registered”，则是 "git.path": "D:\\Program Files\\Git\\bin\\git.exe", 这个位置没有配置正确。

## ref
- https://code.visualstudio.com/docs/editor/integrated-terminal
- https://github.com/Microsoft/vscode/issues/61522
- https://stackoverflow.com/questions/44450218/how-do-i-use-bash-on-ubuntu-on-windows-wsl-for-my-vs-code-terminal
- https://blog.csdn.net/weixin_40965293/article/details/80319982
- https://www.jianshu.com/p/c803d5729f29