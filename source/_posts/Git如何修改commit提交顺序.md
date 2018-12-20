---
title: Git如何修改commit提交顺序
date: 2018-12-16 18:23:30
tags: [git, 'github']
categories: Git
---

> 有时候也会遇到需要把commit的顺序调整的情况，比较小众的需求 记录下

### 前言

`git rebase`的黄金法则便是: 绝不要在公共的分支上使用它

### 分类

* 单个文件，或者单一功能(耦合性低)的commit移动
* 同一个文件的移动(冲突概率比较大)

### 开始

首先我们准备几个提交记录:
可以看到提交的顺序 `add skip merge` `add skip merge2` `add skip merge3`, 我们的目标是颠倒下顺序 `add skip merge3` `add skip merge2` `add skip merge`

![](http://loadingmore-1254319003.coscd.myqcloud.com/skip-merge0.png)

首先进行变基操作 `git rebase -i HEAD~3`, 进入交互页面执行可编辑命令字母`i`
![](http://loadingmore-1254319003.coscd.myqcloud.com/skip-merge1.png)

进行移动操作`command + c` `command + v` or `ctrl + c` `ctrl + v` 等一系列操作后如下:

可以看到已经调了顺序
![](http://loadingmore-1254319003.coscd.myqcloud.com/skip-merge3.png)

退出编辑`esc`,保存`:wq`
显示操作成功

![](http://loadingmore-1254319003.coscd.myqcloud.com/skip-merge4.png)

查看日志`git log`
可以看到已经成功的修改了commit提交顺序

![](http://loadingmore-1254319003.coscd.myqcloud.com/skip-merge5.png)

### 遇到冲突


