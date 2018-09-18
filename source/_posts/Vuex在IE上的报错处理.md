---
title: Vuex在IE上的报错处理
date: 2017-08-09 19:16:07
tags: [Vue]
author: "Yuqi Bi"
---
> 最近在做一个后台项目，其中遇到了vuex在IE浏览器中报错的问题，于是就有了这篇博文进行记录

## 报错语句
```js
SCRIPT5022: [vuex] vuex requires a Promise polyfill in this browser.
```

## 解决办法
1.先安装babel-polyfill
```js
npm install babel-polyfill --save-dev

```
2.build/webpack.base.config.js下修改文件
```js
// 入口文件改为如下
entry: {
  app: ['babel-polyfill','./src/main.js']
}
```
## 原因
IE浏览器没有内置Promise对象。不仅如此，几乎所有的ES6新增的方法在IE都不能用，比如Array.from。因为babel只会转译语法，并不会新增方法
