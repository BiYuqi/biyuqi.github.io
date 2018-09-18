---
title: 《你不知道的javascript中》JS宽松相等和严格相等
date: 2017-06-21 20:43:07
tags: [Javascript]
author: "Yuqi Bi"
header-img: "form-opts.png"
cdn: 'header-on'
---
> 温故而知新，最近在看《你不知道的javascript》系列的书记，看到深处，总有新的发现，好记性不如烂笔头，遂写下这篇读书笔记

## 前言
宽松相等==和严格相等===都用来判断两个值是否"相等",但是他们也有一个很重要的区别，尤其是判断条件上
常见的误区是"==检查值是否相等，===检查值和类型是否相等"。这样理解似乎有点道理，但是还不够准确，正确的解释是：
> "==允许在相等比较中进行强制类型转换，而===不允许"

# 抽象值操作
(名词解释，后面用到再来看也可以)
## ToNumber
ES5 规范在 9.3 节定义了
抽象操作 ToNumber。
其中 true 转换为 1，false 转换为 0。undefined 转换为 NaN，null 转换为 0。
## ToString
定义了抽象操作 ToString，它负责处理非字符串到字符串的强制类型转换。
基本类型值的字符串化规则为：null 转换为 "null"，undefined 转换为 "undefined"，true
转换为 "true"。数字的字符串化则遵循通用规则
## ToPrimitive
为了将值转换为相应的基本类型值，抽象操作 ToPrimitive（参见 ES5 规范 9.1 节）会首先
（通过内部操作 DefaultValue，参见 ES5 规范 8.12.8 节）检查该值是否有 valueOf() 方法。
如果有并且返回基本类型值，就使用该值进行强制类型转换。如果没有就使用 toString()
的返回值（如果存在）来进行强制类型转换。
如果 valueOf() 和 toString() 均不返回基本类型值，会产生 TypeError 错误
# 抽象相等

## 1.字符串和数字之间的相等比较
```js
var a = 42；
var b = "42";

a === b //false
a == b //true
```
因为没有强制类型的转换，所以a===b为false,42不等于"42"
a == b 是宽松相等,如果值得类型不同，则对其中之一或者两者进行强制类型转换。
具体怎么转环？还要看规范：
ES5规范汇这样定义：

**(1) 如果Type(x) 是数字，Type(y) 是字符串，则返回 x == ToNumber(y)的结果。**

**(2) 如果Type(x)是字符串，Type(y)是数字，则返回ToNumber(x) == y的结果**

## 2.其他类型和布尔类型之间的相等比较
== 最容易出错的一个地方时true和false与其他类型之间的相等比较。
```js
var a = "42";
var b = true;

a == b // false
```
根据以往的经验，"42"是一个真值，为什么==的结果不是true呢？原因及简单又复杂，很容易掉坑里，看看规范怎么说：

**(1) 如果Type(x)是布尔类型，则返回ToNumber(x) == y的结果 **

**(2) 如果Type(y)是布尔类型，则返回x == ToNumber(y)的结果**
看个例子
```js
var x = true
var y = "42"

x == y //false
```
Type(x)是布尔值，所以准换后为1，变为1 == "42",两者类型仍然不通，进而"42"继续转换为42，最后变为1 == 42，结果是false，
反过来也一样
```js
var x = '42'
var y = false

x == y //false
```
一个值既不等于true,也不等于false.太奇怪了吧？

这个问题本身就是错误的，我们被自己的大脑欺骗了。

"42" 是一个真值没错，但 "42" == true 中并没有发生布尔值的比较和强制类型转换。

这里不是 "42" 转换为布尔值（true） ，而是 true 转换为 1，"42" 转换为 42。

这里并不涉及 ToBoolean，所以 "42" 是真值还是假值与 == 本身没有关系！
重点是我们要搞清楚 == 对不同的类型组合怎样处理。== 两边的布尔值会被强制类型转换为数字。

很奇怪吧？建议无论什么情况下都不要使用 == true 和 == false。
请注意，这里说的只是 ==，=== true 和 === false 不允许强制类型转换，所以并不涉及ToNumber。
```js
var a = '42'

// 不要这样用，条件不成立
if(a == true){
    //
}

// 也不要这样
if(a === true){
    //
}

//这样显式用法没有问题
if(a){
    //
}

// 这样的显式更好
if(!!a){
    //
}

```
避免了 == true 和 == false(也叫作布尔值的宽松相等)之后就不用担心这些坑了
## 3.null和undefined之间的相等比较
null 和 unfined之间的== 也涉及隐式强制类型的转换：

**(1)如果x为null,y为unfined,这结果为true**

**(2)如果x为unfined，y为null,则结果为true**

在==中null和unfined相等(他们也与其自身相等),除此之外其他值不存在这种情况
```js
var a = nul;
var b;

a == b //true
a == null // true
b == null //true

a == false  //false
b == false //false
a == ''  //false
b == ''  //fasle
a == 0  //false
b == 0 //false
```
null 和 undefined 之间的强制类型转换是安全可靠的，上例中除 null 和 undefined 以外的

其他值均无法得到假阳（false positive）结果。个人认为通过这种方式将 null 和 undefined
作为等价值来处理比较好。
ex:
```js
var a = doSomething()

if(a == null){
    //...
}
```
条件判断a ==null仅仅在函数返回null和undefined时才成立，除此之外都不成立

## 4.对象和非对象之间的相等比较
ES5 规范 11.9.3.8-9 做如下规定：

**(1)如果Type(x)是字符串或者数字，Type(y)是对象，则返回x == ToPromitive(y)的结果**

**(2)如果 Type(x) 是对象，Type(y) 是字符串或数字，则返回 ToPromitive(x) == y 的结果**

```js
var a = 42
var b = [42]
a == b //true
```
[ 42 ] 首先调用 ToPromitive 抽象操作 ，返回 "42"，变成 "42" == 42，然后
又变成 42 == 42，最后二者相等

# 资料
[JavaScript 中的相等性判断|MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Equality_comparisons_and_sameness)
