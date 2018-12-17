---
title: Git如何修改最新commit的message
date: 2018-12-17 23:17:30
tags: [git, 'github']
categories: Git
---
> 项目开发中经常会有修改已经提交commit信息的情况,这里做分析下修改最后一次的提交记录.

### 准备commit提交信息

这里我们准备了两个提交记录: `I am the second commit on edit-new-commit branch`, `I am the first commit on edit-new-commit branch`.

![](http://loadingmore-1254319003.coscd.myqcloud.com/edit-new-commit0.png)

我们准备把最后一条记录`I am the second commit on edit-new-commit branch`修改为`I am the last commit on edit-new-commit branch`.  即`second` > `last`.

### 执行命令

首先我们执行命令:

`git commit --amend`

进入到如下界面：
![](http://loadingmore-1254319003.coscd.myqcloud.com/edit-new-commit1.png)

执行回车，会看到如下页面, 即最后一次commit的界面
![](http://loadingmore-1254319003.coscd.myqcloud.com/edit-new-commit2.png)

然后回到熟悉的节奏, 进入编辑状态`i`, 然后把信息修改即可

![修改后的界面](http://loadingmore-1254319003.coscd.myqcloud.com/edit-new-commit3.png)

然后退出`esc`, 保存`:wq`, 回车即可

### 查看

成功操作:
![](http://loadingmore-1254319003.coscd.myqcloud.com/edit-new-commit4.png)

最后查看我们的log验证, 已经成功修改
![](http://loadingmore-1254319003.coscd.myqcloud.com/edit-new-commit5.png)

### 提交
最后的提交
`git push origin <your branch name>`

Note: 
如果是修改server端最后一条记录，则需要执行最后的提交
`git push origin <your branch name> -f`


