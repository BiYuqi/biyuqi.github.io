---
title: JavaScript获取CSS样式遇到的一些问题
date: 2017-09-25 20:09:48
tags: [Javascript,'CSS']
author: "LoadingMore"
---
> 最近在写一个效果需要获取一个元素translateX的值，因为没有采用JQ,所以用原生方法


## 获取不到样式
```js
const res = document.querySelector('.target').style.width

console.log(res) // 结果为空
```
郁闷啊。。。
突然想到之前看高程的时候看到过getComputedStyle这个方法，于是MDN了一下[这里](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/getComputedStyle)

```js
style                  //只能获取元素的内联样式，内部样式和外部样式使用style是获取不到的。
currentStyle         //适用于IE8及以下。
getComputedStyle     //同currentStyle作用相同，但是适用于FF、opera、safari、chrome IE9+。
```
## getComputedStyle
getComputedStyle 是一个可以获取当前元素所有最终使用的CSS属性值。返回的是一个CSS样式声明对象([object CSSStyleDeclaration])，只读

语法：
```js
let style = window.getComputedStyle(element, [pseudoElt]);

// 用于获取计算样式的Element
// pseudoElt 可选 指定一个要匹配的伪元素的字符串
```

## 兼容性写法
```js
const getStyle = function(obj,attr){
    return obj.currentStyle ? obj.currentStyle[attr] : window.getComputedStyle($(obj))[attr]
}
```
## Javascript获取transform中的属性值
```js
const $ = function(el){
    return document.querySelector(el)
}
const getStyle = function(obj,attr){
    return obj.currentStyle ? obj.currentStyle[attr] : window.getComputedStyle(obj)[attr]
}
const getTransform = function(data){
    const reg = /matrix\((.*)\)/
    const result = reg.exec(data)[1].split(',')
    return {
        transX: parseInt(result[result.length-2]),
        transY: parseInt(result[result.length-1])
    }
}
getTransform(getStyle($('.box'),'transform'))
// {transX: -4, transY: -8}
```
