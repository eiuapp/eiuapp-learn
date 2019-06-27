+++
title = "gitbook（笔记）"
date = 2019-02-15T00:00:00+08:00
lastmod = 2019-02-17T22:48:36+08:00
tags = ["gitbook", "note"]
categories = ["gitbook"]
draft = false
weight = 3001
+++

已移至 gitbook-handbook 仓库

## 入门 {#入门}

-   <https://www.jianshu.com/p/fa38ef97431d>
-   <https://segmentfault.com/a/1190000011440899>
-   <https://www.jianshu.com/p/b0a11b9b8725>
-   <https://www.jianshu.com/p/09a1cac0a0d0>


## 初步 {#初步}

-   <http://www.ituring.com.cn/article/127744>
-   <http://www.chengweiyang.cn/gitbook/github-pages/README.html>


## 进阶 {#进阶}

-   <https://github.com/zhangjikai/gitbook-use/>


## 常见问题 {#常见问题}


### Cannot find module 'prismjs/components/prism-shell.js {#cannot-find-module-prismjs-components-prism-shell-dot-js}

```shell
➜  swift-docs git:(master) ✗ gitbook build ./
Failed to load prism syntax: shell
{ Error: Cannot find module 'prismjs/components/prism-shell.js'
  at Function.Module._resolveFilename (internal/modules/cjs/loader.js:613:15)
  at Function.Module._load (internal/modules/cjs/loader.js:539:25)
  at Module.require (internal/modules/cjs/loader.js:667:17)
  at require (internal/modules/cjs/helpers.js:20:18)
  at requireSyntax (/Users/tomtsang/gitbook/swift-docs/node_modules/gitbook-plugin-prism/index.js:31:3)
  at Object.code (/Users/tomtsang/gitbook/swift-docs/node_modules/gitbook-plugin-prism/index.js:103:11)
  at Record.TemplateBlock.applyBlock (/Users/tomtsang/.gitbook/versions/3.2.3/lib/models/templateBlock.js:205:23)
  at /Users/tomtsang/.gitbook/versions/3.2.3/lib/output/getModifiers.js:56:33
  at /Users/tomtsang/.gitbook/versions/3.2.3/lib/output/modifiers/highlightCode.js:47:24
  at /Users/tomtsang/.gitbook/versions/3.2.3/lib/output/modifiers/editHTMLElement.js:11:16 code: 'MODULE_NOT_FOUND' }
info: >> generation finished with success in 1.8s !
➜  swift-docs git:(master) ✗
```

根据报错信息，查看\`/node\_modules/gitbook-plugin-prism/index.js\`文件及[gitbook-plugin-prism插件配置](<https://github.com/gaearon/gitbook-plugin-prism#lang>)可以知道，插件默认不支持以\`shell\`语法前缀作为\`bash\`语法前缀的别名，所以会报错，但是支持自定义，添加上以下配置即可：

```json
"pluginsConfig": {
    "prism": {
        "lang": {
            "shell": "bash"
        }
    }
}
```


## Ref {#ref}

-   <https://www.imooc.com/article/details/id/30528>
-   <https://github.com/flutterchina/flutter-in-action/pull/111>
