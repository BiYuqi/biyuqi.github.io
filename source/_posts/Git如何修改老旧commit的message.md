---
title: Git如何修改老旧commit的message
date: 2018-12-18 20:30:30
tags: [git, 'github']
categories: Git
---

> 项目开发中经常会有修改已经提交commit信息的情况,这里做分析下修改老旧commit(非最新)的提交记录.

### 前言

`git rebase`的黄金法则便是: 绝不要在公共的分支上使用它

### 准备commit提交信息

老规矩，先准备几条commit信息

![](http://loadingmore-1254319003.coscd.myqcloud.com/edit-old0.png)

然后执行命令`git rebase -i HEAD~3`进入修改模式

![](http://loadingmore-1254319003.coscd.myqcloud.com/edit-old1.png)

接下来我们修改第一次提交的commit信息`Change the eslint config`, 我们改为`Change the eslint base config`

进入编辑`i`模式, 注意一行注释命令`# e, edit = use commit, but stop for amending
` 用edit来实现我们的功能

![](http://loadingmore-1254319003.coscd.myqcloud.com/edit-old2.png)

退出`esc`, `:wq`保存

会弹出以下信息

![](http://loadingmore-1254319003.coscd.myqcloud.com/edit-old3.png)

不要惊慌，我们根据提示进行操作
`You can amend the commit now, with git commit --amend`, 意思是如果要修改commit, 那么就执行这个命令吧.

> 当然了，如果你是手抖，或者不想修改了就执行 `Once you are satisfied with your changes, run git rebase --continue`

![执行--ammend](http://loadingmore-1254319003.coscd.myqcloud.com/edit-old4.png)

然后回到熟悉的节奏
![有木有很熟悉](http://loadingmore-1254319003.coscd.myqcloud.com/edit-old5.png)

### 开始修改
接下来按照最初的设想开始修改吧

照旧执行`i`，进入编辑模式修改

![](http://loadingmore-1254319003.coscd.myqcloud.com/edit-old6.png)

完了,退出`esc`,保存`:wq`

![显示已经修改成功](http://loadingmore-1254319003.coscd.myqcloud.com/edit-old7.png)

现在已经commit，但是rebase操作还没结束。若要通知这个提交的操作已经结束，请指定 --continue选项执行rebase。
`git rebase --continue`

![](http://loadingmore-1254319003.coscd.myqcloud.com/edit-old8.png)

至此我们修改大业才算完成。

### 验收
打个log验收下成果吧
![](http://loadingmore-1254319003.coscd.myqcloud.com/edit-old9.png)


### 最后

提交代码

`git push origin <your branch>`

如果修改的是server端需要加`-f`

`git push origin <your branch> -f`

> `-f` 是 --force的缩写




