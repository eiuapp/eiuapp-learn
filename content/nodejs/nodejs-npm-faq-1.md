---
title: "Nodejs Npm Faq 1"
date: 2018-03-02T15:06:48+08:00
draft: false
tags: ["nodejs", "faq"]
categories: ["nodejs"]
subtitle: ""
descriptions: ""
bigimg:
---

nodejs系列之npm报错

## 报错

#### 报错1，`permission denied, open '/usr/local/lib/node_modules/npm/lib/`

    tom@adata:~/in/ttt$ npm i
    npm ERR! Linux 4.4.0-47-generic
    npm ERR! argv "/usr/local/bin/node" "/usr/local/bin/npm" "i"
    npm ERR! node v6.9.1
    npm ERR! npm  v3.10.8
    npm ERR! path /usr/local/lib/node_modules/npm/lib/fetch-package-metadata.js
    npm ERR! code EACCES
    npm ERR! errno -13
    npm ERR! syscall open

    npm ERR! Error: EACCES: permission denied, open '/usr/local/lib/node_modules/npm/lib/fetch-package-metadata.js'
    npm ERR!     at Error (native)
    npm ERR!     at Object.fs.openSync (fs.js:640:18)
    npm ERR!     at Object.fs.readFileSync (fs.js:508:33)
    npm ERR!     at Object.Module._extensions..js (module.js:578:20)
    npm ERR!     at Module.load (module.js:487:32)
    npm ERR!     at tryModuleLoad (module.js:446:12)
    npm ERR!     at Function.Module._load (module.js:438:3)
    npm ERR!     at Module.require (module.js:497:17)
    npm ERR!     at require (internal/module.js:20:19)
    npm ERR!     at Object.<anonymous> (/usr/local/lib/node_modules/npm/lib/install/deps.js:15:28)
    npm ERR!  { Error: EACCES: permission denied, open '/usr/local/lib/node_modules/npm/lib/fetch-package-metadata.js'
    npm ERR!     at Error (native)
    npm ERR!     at Object.fs.openSync (fs.js:640:18)
    npm ERR!     at Object.fs.readFileSync (fs.js:508:33)
    npm ERR!     at Object.Module._extensions..js (module.js:578:20)
    npm ERR!     at Module.load (module.js:487:32)
    npm ERR!     at tryModuleLoad (module.js:446:12)
    npm ERR!     at Function.Module._load (module.js:438:3)
    npm ERR!     at Module.require (module.js:497:17)
    npm ERR!     at require (internal/module.js:20:19)
    npm ERR!     at Object.<anonymous> (/usr/local/lib/node_modules/npm/lib/install/deps.js:15:28)
    npm ERR!   errno: -13,
    npm ERR!   code: 'EACCES',
    npm ERR!   syscall: 'open',
    npm ERR!   path: '/usr/local/lib/node_modules/npm/lib/fetch-package-metadata.js' }
    npm ERR!
    npm ERR! Please try running this command again as root/Administrator.

    npm ERR! Please include the following file with any support request:
    npm ERR!     /home/tom/in/ttt/npm-debug.log
    tom@adata:~/in/ttt$_

解决：

先查node，npm版本

    tom@adata:~/in/ttt$ node -v
    v6.9.1
    tom@adata:~/in/ttt$ ls -l /usr/local/sbin/bin/n*
    ls: cannot access '/usr/local/sbin/bin/n*': No such file or directory
    tom@adata:~/in/ttt$ which node
    /usr/local/bin/node
    tom@adata:~/in/ttt$ ls -l /usr/local/bin/n*
    -rwxr-xr-x 1 root root 30194989 11月 24 09:45 /usr/local/bin/node
    lrwxrwxrwx 1 root root       38 11月 24 09:45 /usr/local/bin/npm -> ../lib/node_modules/npm/bin/npm-cli.js
    tom@adata:~/in/ttt$

发现，npm是一个软链接，所以，可能问题出在这里，这样呢，我们更新一下 npm 本身吧。`sudo npm i npm -g`。走起。

    tom@adata:~/in/ttt$ sudo npm i npm -g
    [sudo] password for tom:
    /usr/local/bin/npm -> /usr/local/lib/node_modules/npm/bin/npm-cli.js
    - retry@0.10.0 node_modules/npm/node_modules/npm-registry-client/node_modules/retry
    - core-util-is@1.0.2 node_modules/npm/node_modules/request/node_modules/bl/node_modules/readable-stream/node_modules/core-util-is
    - isarray@1.0.0 node_modules/npm/node_modules/request/node_modules/bl/node_modules/readable-stream/node_modules/isarray
    - process-nextick-args@1.0.7 node_modules/npm/node_modules/request/node_modules/bl/node_modules/readable-stream/node_modules/process-nextick-args
    - string_decoder@0.10.31 node_modules/npm/node_modules/request/node_modules/bl/node_modules/readable-stream/node_modules/string_decoder
    - util-deprecate@1.0.2 node_modules/npm/node_modules/request/node_modules/bl/node_modules/readable-stream/node_modules/util-deprecate
    - readable-stream@2.0.6 node_modules/npm/node_modules/request/node_modules/bl/node_modules/readable-stream
    - bl@1.1.2 node_modules/npm/node_modules/request/node_modules/bl
    - async@1.5.2 node_modules/npm/node_modules/request/node_modules/form-data/node_modules/async
    /usr/local/lib
    └─┬ npm@4.0.2
      ├── asap@2.0.5
      ├── config-chain@1.1.11
    ...
    ...
    ...
    tom@adata:~/in/ttt$

我们重新再尝试吧。

    tom@adata:~/in/ttt$ ls
    package.json
    tom@adata:~/in/ttt$ npm i
    openQuote2@2.0.0 /home/tom/in/ttt
    ├── async@1.5.2
    ├─┬ mongodb@2.2.12
    │ ├── es6-promise@3.2.1
    │ ├─┬ mongodb-core@2.0.14
    │ │ ├── bson@0.5.7
    │ │ └─┬ require_optional@1.0.0
    │ │   ├── resolve-from@2.0.0
    │ │   └── semver@5.3.0
    │ └─┬ readable-stream@2.1.5
    │   ├── buffer-shims@1.0.0
    │   ├── core-util-is@1.0.2
    │   ├── inherits@2.0.3
    │   ├── isarray@1.0.0
    │   ├── process-nextick-args@1.0.7
    │   ├── string_decoder@0.10.31
    │   └── util-deprecate@1.0.2
    └─┬ mysql@2.12.0
      ├── bignumber.js@2.4.0
      ├─┬ readable-stream@1.1.14
      │ └── isarray@0.0.1
      └── sqlstring@2.2.0

    tom@adata:~/in/ttt$

好了，成功了。
但是，我重新再去查

    tom@adata:~/in/ttt$ which npm
    /usr/local/bin/npm
    tom@adata:~/in/ttt$ which node
    /usr/local/bin/node
    tom@adata:~/in/ttt$ ls -l /usr/local/bin/n*
    -rwxr-xr-x 1 root root 30194989 11月 24 09:45 /usr/local/bin/node
    lrwxrwxrwx 1 root root       38 11月 24 09:45 /usr/local/bin/npm -> ../lib/node_modules/npm/bin/npm-cli.js
    tom@adata:~/in/ttt$
    tom@adata:~/in/ttt$ npm -v
    4.0.2
    tom@adata:~/in/ttt$

发现只更新了 npm 的版本，而已呀。看来，升级npm，才是解决这个问题的关键。
