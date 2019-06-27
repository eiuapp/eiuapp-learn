
## gvm 安装 go1.5+ 必先 gvm install go1.4 -B

```
➜  ~ git:(master) ✗ gvm install go1.11.5
Downloading Go source...
Installing go1.11.5...
 * Compiling...
/root/.gvm/scripts/install: 行 84: go: 未找到命令
ERROR: Failed to compile. Check the logs at /root/.gvm/logs/go-go1.11.5-compile.log
ERROR: Failed to use installed version
➜  ~ git:(master) ✗ 
```

参考 https://github.com/moovweb/gvm/issues/302 到 https://github.com/moovweb/gvm#a-note-on-compiling-go-15 

```
gvm install go1.4 -B
gvm use go1.4
export GOROOT_BOOTSTRAP=$GOROOT
gvm install go1.5
```

## ref
- https://github.com/moovweb/gvm#a-note-on-compiling-go-15 