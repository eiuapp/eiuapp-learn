+++
title = "Can scp create a directory if it doesn't exist?"
date = 2019-01-19T00:00:00-08:00
lastmod = 2019-01-19T00:44:25-08:00
tags = ["shell", "scp"]
categories = ["shell"]
draft = false
weight = 3001
+++

Can scp create a directory if it doesn't exist?

As far as I know, scp itself cannot do that, no. However, you could just ssh to the target machine, create the directory and then copy. Something like:

```shell
ssh user@host "mkdir -p /target/path/" &&
      scp /path/to/source user@host:/target/path/
```

Note that if you are copying entire directories, the above is not needed. For example, to copy the directory ~/foo to the remote host, you could use the -r (recursive) flag:

```shell
scp ~/foo/ user@host:~/
```

That will create the target directory ~/foo on the remote host.
However, it can't create the parent directory. scp ~/foo user@host:~/bar/foo will fail unless the target directory bar exists.
In any case, the -r flag won't help create a target directory if you are copying individual files.


## Ref {#ref}

-   <https://unix.stackexchange.com/questions/193368/can-scp-create-a-directory-if-it-doesnt-exist>
