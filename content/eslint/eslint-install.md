+++
title = "eslint入门"
date = 2019-02-10T00:00:00+08:00
lastmod = 2019-02-10T18:31:05+08:00
tags = ["eslint", "install"]
categories = ["eslint"]
draft = false
weight = 3001
+++

## 安装 {#安装}

```shell
npm install -g eslint babel-eslint eslint-plugin-react eslint-config-airbnb
```

在安装的时候得注意一点，eslint与eslint-config-airbnb要么都执行全局安装，要么都本地安装，必须相同哟。

然后，把 <https://github.com/airbnb/javascript/blob/master/linters/.eslintrc> 下载到Project 中就可以了。


## 使用 {#使用}

配置完相关信息后，就可以切到项目目录内然后执行检测啦：

我们新建一个test.js进行检测

```shell
$ eslint test.js
```


## JavaScript Style Guide {#javascript-style-guide}

关于 JavaScript Style Guide 可以直接参考

-   <https://github.com/airbnb/javascript>


## react eslint {#react-eslint}

优先查看：

在React+Babel+Webpack环境中使用ESLint

-   <https://www.cnblogs.com/le0zh/p/5619350.html>
-   <https://www.bbsmax.com/A/obzbX0j65E/>

React-native ESLint & Airbnb 配置

-   <https://www.jianshu.com/p/1d66a10466d2>


### npm run lint 检查所有js文件 {#npm-run-lint-检查所有js文件}

在根目录的 package.json文件下修改如下:

```json
"scripts": {
    "lint": "eslint --ext .js ./src --fix"
}
```

根目录下运行:

```shell
npm run lint
```


### 再webpack配置中使用eslint加载器 {#再webpack配置中使用eslint加载器}

添加 到：webpack.cofnig.js

```shell
...
eslint: {
    configFile: './.eslintrc'
},
...
```


### pre-commit钩子 {#pre-commit钩子}

如果项目使用了git,可以通过使用pre-commit钩子在每次提交前检测，如果检测失败则禁止提交。可以在很大一定程度上保证代码质量。

这里我们使用了pre-commitgit包来帮助我们实现这一目标。

首先在package.json中添加script命令：

```json
"scripts": {
    "eslint": "eslint --ext .js src"
}
```

其次，安装pre-commit

```shell
npm install pre-commit --save-dev
```

最后，在package.json中配置pre-commit需要运行的命令：

```json
"pre-commit": [
    "eslint"
]
```

完成之后，在每次提交之前，都会运行eslint命令进行检测，如果检测到有违反代码规则的情况，则会返回1，导致git commit失败。


### airbnb eslint {#airbnb-eslint}

使用airbnb的eslint
<https://www.bbsmax.com/A/Ae5RYomYJQ/>
<https://www.cnblogs.com/samwu/p/5772778.html>


## Ref {#ref}

-   <https://segmentfault.com/a/1190000004965396>
-   <https://github.com/airbnb/javascript>
-   <http://spacemacs.org/layers/+frameworks/react/README.html>
-   <https://eslint.org/docs/user-guide/configuring>
-   <https://csspod.com/getting-started-with-eslint/>
-   <https://www.jianshu.com/p/ad6784d028aa>
-   <http://eslint.cn/docs/user-guide/configuring>
