---
title: Git如何克隆指定的分支
date: 2019-10-12 23:54:16
tags: [Git, single-clone]
categories: Git
---

> 偶尔会遇到只想克隆指定分支的情况，不想下载主分支，所以有了这篇文章.

## 使用

在Git`1.7.10`及更高版本中，添加`--single-branch`以防止提取所有分支.
`<folder>` 可以指定克隆岛某个文件夹
`branchname` 需要克隆的分支名
`url` 远程仓库地址
`--branch` 创建分支， 亦可用`-b`代替

```js
git clone --branch  <branchname> <url> --single-branch [<folder>]
```
