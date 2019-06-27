在 编写 `~/.zshrc` 的过程中，请不要使用 `~`而改为使用 `$HOME`。

请直接看下面示例：

```shell
➜  ~ git:(master) ✗ tail -25 ~/.zshrc
if [ -f "$HOME/.zshrc.local" ]; then 
    echo "hello"
else
    echo "no .zshrc.local"
fi 

if [ -f "~/.zshrc.local" ]; then 
    echo "hello "
else
    echo "no .zshrc.local"
fi 
echo `pwd`
➜  ~ git:(master) ✗ source ~/.zshrc
hello
no .zshrc.local
/home/ubuntu
➜  ~ git:(master) ✗ 
```

使用 `$HOME`，正确。使用 `~`，出错。