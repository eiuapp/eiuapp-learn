---
title: "Emacs Flycheck Eslint"
date: 2018-08-23T15:05:07+08:00
draft: false
tags: ["emacs","flycheck", "eslint"]
categories: ["emacs"]
subtitle: ""
descriptions: ""
bigimg:
---

## Env

- os: mac
- emacs: GNU Emacs 22.1.1


## Step

https://github.com/flycheck/flycheck/issues/1195

遇到下面的情形，就按提示，把这些个包，一个一个地安装。

```
➜  tmp git:(master) ✗ eslint -v
v5.4.0
{
➜  tmp git:(master) ✗ ls
test.js
➜  tmp git:(master) ✗ cat test.js
var a=1;
var b = 2;
vardf c=a+b;

function a(){
    console.log("hello world")

}
➜  tmp git:(master) ✗ eslint test.js

Oops! Something went wrong! :(

ESLint: 5.4.0.
ESLint couldn't find the plugin "eslint-plugin-import". This can happen for a couple different reasons:

1. If ESLint is installed globally, then make sure eslint-plugin-import is also installed globally. A globally-installed ESLint cannot find a locally-installed plugin.

2. If ESLint is installed locally, then it's likely that the plugin isn't installed correctly. Try reinstalling by running the following:

    npm i eslint-plugin-import@latest --save-dev

Path to ESLint package: /usr/local/lib/node_modules/eslint

If you still can't figure out the problem, please stop by https://gitter.im/eslint/eslint to chat with the team.

➜  tmp git:(master) ✗ npm i -g eslint-plugin-import
npm WARN eslint-plugin-import@2.14.0 requires a peer of eslint@2.x - 5.x but none is installed. You must install peer dependencies yourself.

+ eslint-plugin-import@2.14.0
added 49 packages from 32 contributors in 15.936s
➜  tmp git:(master) ✗ eslint test.js

Oops! Something went wrong! :(

ESLint: 5.4.0.
ESLint couldn't find the plugin "eslint-plugin-node". This can happen for a couple different reasons:

1. If ESLint is installed globally, then make sure eslint-plugin-node is also installed globally. A globally-installed ESLint cannot find a locally-installed plugin.

2. If ESLint is installed locally, then it's likely that the plugin isn't installed correctly. Try reinstalling by running the following:

    npm i eslint-plugin-node@latest --save-dev

Path to ESLint package: /usr/local/lib/node_modules/eslint

If you still can't figure out the problem, please stop by https://gitter.im/eslint/eslint to chat with the team.

➜  tmp git:(master) ✗ npm i -g eslint-plugin-node
npm WARN eslint-plugin-node@7.0.1 requires a peer of eslint@>=4.19.1 but none is installed. You must install peer dependencies yourself.
npm WARN eslint-plugin-es@1.3.1 requires a peer of eslint@>=4.19.1 but none is installed. You must install peer dependencies yourself.

+ eslint-plugin-node@7.0.1
added 12 packages from 7 contributors in 6.891s
➜  tmp git:(master) ✗ eslint test.js

Oops! Something went wrong! :(

ESLint: 5.4.0.
ESLint couldn't find the plugin "eslint-plugin-promise". This can happen for a couple different reasons:

1. If ESLint is installed globally, then make sure eslint-plugin-promise is also installed globally. A globally-installed ESLint cannot find a locally-installed plugin.

2. If ESLint is installed locally, then it's likely that the plugin isn't installed correctly. Try reinstalling by running the following:

    npm i eslint-plugin-promise@latest --save-dev

Path to ESLint package: /usr/local/lib/node_modules/eslint

If you still can't figure out the problem, please stop by https://gitter.im/eslint/eslint to chat with the team.

➜  tmp git:(master) ✗ npm i -g eslint-plugin-promise
+ eslint-plugin-promise@4.0.0
added 1 package from 1 contributor in 1.618s
➜  tmp git:(master) ✗ eslint test.js

Oops! Something went wrong! :(

ESLint: 5.4.0.
ESLint couldn't find the plugin "eslint-plugin-standard". This can happen for a couple different reasons:

1. If ESLint is installed globally, then make sure eslint-plugin-standard is also installed globally. A globally-installed ESLint cannot find a locally-installed plugin.

2. If ESLint is installed locally, then it's likely that the plugin isn't installed correctly. Try reinstalling by running the following:

    npm i eslint-plugin-standard@latest --save-dev

Path to ESLint package: /usr/local/lib/node_modules/eslint

If you still can't figure out the problem, please stop by https://gitter.im/eslint/eslint to chat with the team.

➜  tmp git:(master) ✗ npm i -g eslint-plugin-standard
npm WARN eslint-plugin-standard@3.1.0 requires a peer of eslint@>=3.19.0 but none is installed. You must install peer dependencies yourself.

+ eslint-plugin-standard@3.1.0
added 1 package from 1 contributor in 6.621s
➜  tmp git:(master) ✗ eslint test.js

/Users/tomtsang/.emacs.d/test/tmp/test.js
  10:7  error  Parsing error: Unexpected token c

✖ 1 problem (1 error, 0 warnings)

(node:82363) [ESLINT_LEGACY_OBJECT_REST_SPREAD] DeprecationWarning: The 'parserOptions.ecmaFeatures.experimentalObjectRestSpread' option is deprecated. Use 'parserOptions.ecmaVersion' instead. (found in "standard")
➜  tmp git:(master) ✗
```

又如下面这个，也是一样的

```
➜  test eslint test.js
Cannot find module 'eslint-config-airbnb'
Referenced from: /Users/tomtsang/.eslintrc.js
Error: Cannot find module 'eslint-config-airbnb'
Referenced from: /Users/tomtsang/.eslintrc.js
    at ModuleResolver.resolve (/usr/local/lib/node_modules/eslint/lib/util/module-resolver.js:72:19)
    at resolve (/usr/local/lib/node_modules/eslint/lib/config/config-file.js:484:28)
    at load (/usr/local/lib/node_modules/eslint/lib/config/config-file.js:556:26)
{
    at configExtends.reduceRight (/usr/local/lib/node_modules/eslint/lib/config/config-file.js:430:36)
    at Array.reduceRight (<anonymous>)
    at applyExtends (/usr/local/lib/node_modules/eslint/lib/config/config-file.js:408:26)
    at loadFromDisk (/usr/local/lib/node_modules/eslint/lib/config/config-file.js:528:22)
    at Object.load (/usr/local/lib/node_modules/eslint/lib/config/config-file.js:564:20)
    at Config.getPersonalConfig (/usr/local/lib/node_modules/eslint/lib/config.js:154:37)
    at Config.getLocalConfigHierarchy (/usr/local/lib/node_modules/eslint/lib/config.js:248:41)
➜  test
```

## Ref

- https://github.com/flycheck/flycheck/issues/1195
