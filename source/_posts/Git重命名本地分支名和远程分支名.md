---
title: Git重命名本地分支名和远程分支名
date: 2019-06-12 23:30:30
tags: [git, 'github']
categories: Git
---
> 如果你错误的命名了一个分支名并且推送到了远端服务器，你可以在被发现之前得到修正

##### 1.重命名本地分支
如果你在你想要改名的分支上：
```js
git branch -m new-name
```
如果你在其他分支上：
```js
git branch -m old-name new-name
```

##### 2.删除远端分支并推送本地的新分支
```js
git push origin :old-name new-name
```

##### 3.重命名本地分支的上游分支：
切换到该分支然后：
```js
git push origin -u new-name
```

