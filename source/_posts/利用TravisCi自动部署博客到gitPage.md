---
title: 利用TravisCi自动部署博客到gitPage
date: 2018-09-18 17:32:00
tags: ['hexo', 'Travis-CI']
categories: Javascript
---

> 一篇测试文档，测试自动部署gitPage，稍后会写篇博客进行记录,先温习下git

### 创建本地分支
* **git branch 分支名**
  eg: git branch dev 基于当前的分支创建本地分支

### 切换到本地分支
* **git checkout 分支名**
  eg: git checkout dev 切换到dev分支

### 创建并切换到本地分支
* **git checkout -b 分支名**
  eg: git checkout -b dev，这条命令把创建本地分支和切换到该分支的功能结合起来了，即基于当前分支master创建本地分支dev并切换到该分支下

### 删除分支
* **git branch -d 分支名**
  eg: git branch -d dev 删除本地dev分支

### 删除未提交分支
* **git branch -D 分支名**
  eg: git branch -D dev 强制删除本地dev分支

### 合并分支
合并dev到master
* **git checkout master**  # 切换到master分支
* **git merge 分支名** # 合并该分支到master

### 提交本地分支到远程仓库
* **git push origin 远程仓库名**
  eg: git push origin dev，这条命令表示把本地dev分支提交到远程仓库，即创建了远程分支dev

## 基于远程分支新建本地分支
* **git checkout -b 本地分支 origin/远程分支**
  eg: git checkout -b dev origin/dev

### 新建本地分支与远程分支关联
* **git branch --set-upstream dev origin/dev**
  eg: git branch –set-upstream dev origin/dev，把本地dev分支和远程dev分支相关联

注意：本地新建分支， push到远程服务器上之后，使用git pull或者git pull 拉取或提交数据时会报错，必须使用命令：git pull origin dev（指定远程分支）；如果想直接使用git pull或git push拉去提交数据就必须创建本地分支与远程分支的关联






