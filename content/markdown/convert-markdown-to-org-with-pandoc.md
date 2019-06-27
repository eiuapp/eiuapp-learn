convert-markdown-to-org-with-pandoc

## pandoc

检查是否有 pandoc 命令

### install
```shell
docker pull dalibo/pandocker
alias pandoc="docker run --rm -u `id -u`:`id -g` -v `pwd`:/pandoc dalibo/pandocker"
```


## CRLF or LF

### 查看文件类型

使用vim打开文件，输入:set ff?。根据返回结果可以文件类型

- https://www.cnblogs.com/jiangz/p/4231279.html

### LR

把文档全转成 unix end-of-line

```shell
dos2unix ./*/*.md
```

- https://github.com/greenrobot/greenDAO/issues/71


## to org

```shell
docker run --rm -u `id -u`:`id -g` -v `pwd`:/pandoc dalibo/pandocker README.md -o README.org
```

或者

```shell
pandoc README.md -o README.org
```

## ref
- https://pandoc.org/demos.html
- https://hub.docker.com/r/dalibo/pandocker
