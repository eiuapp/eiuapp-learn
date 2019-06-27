---
title: "Nodejs Faq Unexpected Token Function"
date: 2018-02-23T15:42:10+08:00
draft: false
tags: ["nodejs", "faq"]
categories: ["nodejs"]
subtitle: ""
descriptions: ""
bigimg:
---

## env 

在 `hugo-algolia -s`命令运行时, 出现了 `Unexpected token function` 报错

```
➜  tomtsang-rootsongjc-hugo git:(master) ✗ hugo-algolia -s
/usr/local/lib/node_modules/hugo-algolia/lib/utils.js:11
async function copySynonyms(fromIndex, toIndex) {
      ^^^^^^^^
SyntaxError: Unexpected token function
    at Object.exports.runInThisContext (vm.js:78:16)
    at Module._compile (module.js:543:28)
    at Object.Module._extensions..js (module.js:580:10)
    at Module.load (module.js:488:32)
    at tryModuleLoad (module.js:447:12)
    at Function.Module._load (module.js:439:3)
    at Module.require (module.js:498:17)
    at require (internal/module.js:20:19)
    at Object.<anonymous> (/usr/local/lib/node_modules/hugo-algolia/lib/index.js:11:52)
    at Module._compile (module.js:571:32)
➜  tomtsang-rootsongjc-hugo git:(master) ✗ 
```

## step

`Unexpected token function` 报错, 是指未预期的token 函数, 那先就要查一下, nodejs版本支持与否

```
➜  tomtsang-rootsongjc-hugo git:(master) ✗ node -v
v7.2.1
```

报错信息, 知函数为 `async function`

```
➜  tomtsang-rootsongjc-hugo git:(master) ✗ head -n 20 /usr/local/lib/node_modules/hugo-algolia/lib/utils.js
function copySettings(fromIndex, toIndex) {
    const settings = fromIndex.getSettings();

    if(settings['replicas'] !== undefined) {
        settings['replicas'] = undefined;
    }

    toIndex.setSettings(settings);
}

async function copySynonyms(fromIndex, toIndex) {
    let page = 0;

    do {
        let results =  await fromIndex.searchSynonyms({
            query: '',
            type: 'synonym,oneWaySynonym',
            page,
        });

➜  tomtsang-rootsongjc-hugo git:(master) ✗
```

通过 [Node.js ES2015 Support](http://node.green), 知道, `async function` 最少需要 nodejs 在 7.10.1 版本. 所以这个问题就是去升级版本吧

## check

```
➜  tomtsang-rootsongjc-hugo git:(master) ✗ node -v
v9.5.0
➜  tomtsang-rootsongjc-hugo git:(master) ✗ npm -v
5.6.0
➜  tomtsang-rootsongjc-hugo git:(master) ✗ hugo-algolia -s
JSON index file was created in public/algolia.json
{ updatedAt: '2018-02-23T08:04:32.198Z', taskID: 3509864752 }
➜  tomtsang-rootsongjc-hugo git:(master) ✗
```

## ref

- https://github.com/10Dimensional/hugo-algolia/issues/1
- [Node.js ES2017 Support](http://node.green/#ES2017-features-async-functions)
