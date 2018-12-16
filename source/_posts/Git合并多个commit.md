---
title: Git合并多个commit
date: 2018-12-16 11:30:30
tags: [git, 'github']
categories: Git
---

> 项目开发中，很多时候都是单独开一个新的分支，完成一个模块，在该分支上由于功能较大，或者保险起见，我们可能会多次commit我们的文件,但是在提交的时候，我们不希望一个功能出现多次commit,导致review代码不便，所以经常在提交前，合并下我们的commit记录. 特此记录

### 假设我们有四个commit

`git log` 查看提交日志.

> 注: `glgg` 命令为`oh-my-zsh`附带aliases, 如果没有安装可自动忽略, 采用git原生操作即可, 下面不多做解释

![这是图片位置](http://loadingmore-1254319003.file.myqcloud.com/git-commit-compose.png)

### 通过 git rebase -i <commit hash> 执行合并操作

* 我们将commit为 `Add four part` `Add third part` `Add two part` 合并到 `Add first part`
* 并修改最后的commit信息

![这是执行命令的图片](http://loadingmore-1254319003.file.myqcloud.com/git-rebase-i-hash.png)

> 进入rebase时可以指定一个commit范围，比如：
> git rebase -i HEAD~5. 这样也是可行的

解释下，-i选项用来交互式地运行变基, 必须指定想要重写多久远的历史，即指定commit hash, 本例是指定到第一条commit hash 接着我们就进入到vim的编辑模式

> --interactive let the user edit the list of commits to rebase

> vim下 i是进入编辑模式, esc退出编辑模式, :wq 退出并保存

![进入vim编辑模式图片](http://loadingmore-1254319003.file.myqcloud.com/git-rebase-i-vim.png)

图中上半部分为主注释的是可编辑部分,下半部分是指令的说明. 由指令名称, commit hash, commit message组成

### 修改指令

`squash`命令会合并到前一个commit

![选中s](http://loadingmore-1254319003.file.myqcloud.com/git-squash.png)

命令行中`pick` 改为 `s`或`squash` 然后保存退出(esc, :wq)回车即可

再次出现命令行，提示让重新修改提交commit message
![](http://loadingmore-1254319003.file.myqcloud.com/git-tip-edit.png)

其中, 非注释部分就是三次的 commit message, 你要做的就是删除最后留一个,写成你想提交的message, 保存退出即可

该处我们修改为: `Add compose feature`

保存退出(esc, :wq)回车 显示如下， 已经成功修改

![](http://loadingmore-1254319003.file.myqcloud.com/git-edit-rename.png)

查看下本地是否合并：
![](http://loadingmore-1254319003.file.myqcloud.com/git-last.png)
可以看到已经修改成功了，write-blog分支上只有一条提交记录了

最后：
`git push origin <your branch>`

注意：
如果该分支上是已经提交server的commit,提交的时候需要 `git push origin <your branch> -f`

