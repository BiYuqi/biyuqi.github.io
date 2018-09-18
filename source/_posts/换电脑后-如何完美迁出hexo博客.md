---
title: '换电脑后,如何完美迁出hexo博客'
date: 2017-06-21 22:25:48
tags: [javascript,hexo]
author: "Yuqi Bi"
header-img: "form-opts.png"
cdn: 'header-on'
---
> 最近换了电脑，之前电脑上搭建的静态博客也需要迁移了，上网查了下资料，自己也着手成功的迁移到了新的电脑

## 开始前准备
本博文默认git,nodejs已经安装好
## 分析文件
### 1.哪些文件是必须拷贝走的(拷贝到新的电脑)
首先是之前自己修改的文件，像配置文件_config.yml,theme文件夹，source文件夹自己写的原始文件这些都是必须要拷贝走的。除此之外还有scaffolds文件夹(文件的模板)，package.json(使用哪些包)，.gitignore(提交忽略哪些文件夹)
总结：

* _config.yml
* theme
* source
* scaffolds
* package.json
* .gitignore

** 这些是需要拷贝的 **

### 2.哪些文件是需要忽略不用管的

.git node_mouldes/ public/ .deploy_git/ db.json

## 开始迁移
```js
// 全局安装hexo
npm install hexo-cli -g

// 把必须拷贝的文件，拷贝到新建的文件夹内，执行以下命令
// 在新建的文件夹内打开命令行，安装必要的模块，初始化
// 这里不用hexo init初始化，因为配置文件我们已经拷贝过来(一定要慎重，严格按照教程来)

npm install

// 安装其他一些必要的组件
npm install hexo-deployer-git --save

npm install hexo-generator-feed --save

npm install hexo-generator-sitemap --save
```
## 测试是否安装成功
```js
//先本地预览
hexo clean

hexo g

hexo s

//此时可以先在localhost:4000 本地预览博客，如果不报错，那就说明迁移成功

// 正式部署
hexo clean

hexo g

hexo d
```
至此，可以打开网页看看部署情况，祝各位成功，有问题随时联系我 biyuqiwan@163.com
