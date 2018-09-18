---
title: '解决Vue复用组件切换时组件不更新的问题'
date: 2018-04-12 00:15:48
tags: [vue-router, Vue, 路由]
author: "Yuqi Bi"
header-img: "form-opts.png"
cdn: 'header-on'
---
> 距离上次更新已经是去年了,习惯这个东西真可怕，本文仅做个小记录

## 背景
做项目中,复用的vue组件，切换时，组件的生命周期都不执行了,查资料得知是vue的组件复用机制问题，相同的组件会被复用，也就不存在更新了
## 解决方案
在router-view上加上一个动态key属性值
```html
<router-view :key="key"/>
```
```js
computed: {
  key () {
    return this.$route.name !== undefined ? this.$route.name + +new Date() : this.$route + +new Date()
  }
}
```
欧克...(逃)
