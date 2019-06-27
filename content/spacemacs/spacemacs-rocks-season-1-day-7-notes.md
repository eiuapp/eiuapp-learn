+++
title = "Spacemacs Rocks Day 7, Spacemacs buffer, file, project and layout management"
date = 2018-11-17T11:00:00+08:00
lastmod = 2018-11-19T01:44:14+08:00
tags = ["emacs", "spacemacs"]
categories = ["spacemacs"]
draft = false
weight = 3009
+++

[youku](http://v.youku.com/v%5Fshow/id%5FXMTM5NDI3NjQxMg==.html?spm=a2h1n.8251843.playList.5!9~5~A&f=26137579&o=1)


## SPC f  --->    file related operations {#spc-f-file-related-operations}

```markdown
SPC f f  --> helm-find-file
SPC f r  --> open recent file
SPC f R  --> rename file
SPC f c  --> copy file
SPC f j  --> jump to dired
SPC f t  --> open neo tree
SPC f o  --> open in external application
```

在 SPC f f的时候，可以通过 TAB 和 C-h 补充和回退文件夹名。


## SPC b --->  buffer related operations {#spc-b-buffer-related-operations}

```markdown
SPC b b & SPC b B(i)
SPC b h (spacemacs home buffer)
SPC b s (scratch buffer)
SPC b f (reveal in finder)
SPC b w in dired buffer.
SPC b n/p (previous or next buffer)
SPC b TAB to switch back and forth.
```

SPC b w in dired buffer.从而实现对文件名的修改, 多行编辑等操作。


## SPC p  ---> project related operations {#spc-p-project-related-operations}

```markdown
SPC p f  and SPC p b
```


## SPC l ---> layout replated operations {#spc-l-layout-replated-operations}

```markdown
SPC l o  --> custom layout
SPC l L/s --> load or save layout
SPC l l --> switch bewteen layout
SPC l TAB --> quick way to switch
SPC l ?  --> open up the help.
SPC p l --> switch to project and create a layout
```


## dired related operations {#dired-related-operations}

```markdown
new file/delete file/rename file
new folder/delete folder/rename folder
```

\`+\`号,新建文件夹。
在文件夹上，f 新建文件。


### 标记并删除 {#标记并删除}

在 dired 模式下，m 标记，D 删除。


## Ranger {#ranger}

```markdown
SPC a r
h l to navigate folder
j k to preview file
```
