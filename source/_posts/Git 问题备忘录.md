---
title: Git问题备忘录
date: 2019-10-01 23:30:30
tags: [git, 'github']
categories: Git
---

> 日常开发，难免会遇到各种Git问题，特此记录.

- fatal: Unable to create '/path/my_project/.git/index.lock': File exists

```bash
rm -f ./.git/index.lock
```

- 重置所有commit提交信息

```basj
git rebase -i --root
```

- 大小写敏感
```bash
git config core.ignorecase false
```

- 新增远程仓库地址

```bash
git remote add origin [url]
```
