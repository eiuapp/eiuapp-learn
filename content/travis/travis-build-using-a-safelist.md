

travis分支白名单导致无法构建

手动触发

报出错误alert如下：

```
Oh no!
You tried to trigger a build for eiuapp/linux-hugo but the request was rejected.
```

且 提示信息中的 
```
Branch "master" not included per configuration.
```

则是 分支白名单的问题

检看.travis.yml 文件，是否对 branches.only 中设置正确

```yml
# Specify which branches to build using a safelist
# 分支白名单限制: 只有hugo分支的提交才会触发构建
branches:
  only:
    - master 
```