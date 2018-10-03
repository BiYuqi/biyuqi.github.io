---
title: github一个远程仓库的某个分支放到一个新的仓库，保留历史提交记录
date: 2018-10-03 16:18:51
tags: [git]
categories: Git
---
> 记录下迁将一个远程仓库的某个分支放到一个新的仓库中（提交历史纪录也导过去的一点经验

## origin_seed(名字可自取，临时用，迁完后，主动取消关联) 区别于origin 目的在于将当前仓库(分支)新关联到新的仓库

##### 新建一个仓库，假设仓库名为[webpack-seed](https://github.com/BiYuqi/webpack-seed)

##### 切换到旧仓库(将要迁走的仓库下, 分支自己选, 或者默认迁走master)，然后关联新仓库

```js
git remove add origin_seed https://github.com/BiYuqi/webpack-seed.git (你的新仓库地址)
```

##### 推送到新的仓库
* 此处是将旧仓库下 web-ejs-pc 分支推送到新仓库master分支, 分支根据需要自己填写
```js
git -u push origin_seed web-ejs-pc:master
```

##### 取消关联，原有仓库恢复原样

```js
git remote remove origin_seed
```

至此，可以去查看新仓库是否迁移成功.
