+++
title = "Spacemacs Rocks Day 4, Magit workflow"
date = 2018-11-15T11:00:00+08:00
lastmod = 2018-11-20T10:22:55+08:00
tags = ["emacs", "spacemacs", "magit"]
categories = ["spacemacs"]
draft = false
weight = 3006
+++

如果magit状态下，不知道按键，则 SPC g s 后按 ? 号，查看。


## Magit init, commit and commit messages {#magit-init-commit-and-commit-messages}

1.  SPC g i(magit-init)
2.  SPC u S(所有文件加入版本库)
3.  按TAB查看文件的修改
4.  SPC g s(magit-status)
5.  s-1 (goto unstaged section)
6.  press tab to view diffs(code review)
7.  s to stage all changes
8.  c c ->  c-c c-c to commit
9.  editing the commit message and the diff on the right


## Commits in Action {#commits-in-action}

1.  Ammend ( c a)  就是把之前的commit作修改。把最近的修改，加入到上一次的
    commit 之中。
2.  write good commit messages
3.  squash unpushed commits (r l)
4.  reset commits (l l #)
5.  select a few line to commits
6.  discard changes, file wide or line wide
7.  revert commits


### git push {#git-push}

1.  SPC g s
2.  M a(remote name, remote url)
3.  P
4.  第一次的话，按 - u(git push)
5.  按提示，一般是 p或u


### git rebase squash 把2个或者多个 commit ，合并成一个。 {#git-rebase-squash-把2个或者多个-commit-合并成一个}

1.  SPC g s
2.  多个commit, 但是没有 push
3.  找到最前的一个commit(commit 1)
4.  按r i，进入一个commit 列表。
5.  C-c, C-c
6.  按 r 再按s , 表示进入到 squash 状态
7.  按 , c 进入最终的commit Message 编辑
8.  C-c C-c 退出。这时，观察到，2个commit已经合并成一个了。
9.  git push 吧。


### git reset {#git-reset}

-   <https://magit.vc/manual/magit/Resetting.html>

SPC h d f 查找 magit-reset 函数, 知道，是在 git status状态下, 光标到希望reset 的
commit 的地方，然后按 o ，然后回车或者另选择。


### discard unstaged changes {#discard-unstaged-changes}

-   discard 整个修改过的文件，直接在文件处按 x
-   discard 文件中的部分修改，TAB后，按v选中，按x

总之就是先选中，然后按x,然后确认。


### select a few line to commits {#select-a-few-line-to-commits}

1.  press tab to view diffs(code review)
2.  v 选中修改的行
3.  s stage

这样就只选择部分进行 commit . 注意，有些应该是要删除原来的语句的哟。


### revert commits {#revert-commits}

按 -


## Branching basics {#branching-basics}

1.  b B to create branch
2.  m m to merge
3.  b b to switch branch


### create a new branch {#create-a-new-branch}

好像 b B 已经失效

> SPC g s b c


### merge develop branch {#merge-develop-branch}

> SPC g s m m


## Rebase {#rebase}

1.  squash unpushed
2.  rebase to other branch
3.  interactive rebase


## pull, push & send Pull request {#pull-push-and-send-pull-request}

1.  P P
2.  f f / f o to fetch branch
3.  F to pull
4.  s-g to send pull request


## Misc {#misc}

1.  cherry pick
2.  view github files
