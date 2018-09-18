---
title: JS字符串常用方法总结
date: 2017-04-22 21:54:07
tags: [javascript]
categories: Javascript
---

> 周末温习高程三,摘录读书笔记

### 字符方法
* charAt()
* charCodeAt()

<!-- more -->
都接收一个参数，区别是chartAt()方法以单独字符串形式返回给定位置的字符
eg:

```js
var str = 'hello world';
alert(str.charAt(1))//e
```
如果你想得到的不是字符而是编码那就需要charCodeAt():
```js
var str2 = 'hello world';
alert(str2.charCodeAt(1))//101  小写字母e的字符编码
```
注：ECMAScript5中还定义了可以用[Number]的方式访问字符串中特定字符;IE8+
### 字符串操作方法
* concat()
* slice()
* substr()
* substring()

```js
//concat() //顾名思义用来拼接一个或多个字符
var str = 'hello';
var str0 = str.concat('world');
console.log(str0)//'hello world'
console.log(str)//'hello'
//实践中还是用+号操作符的拼接字符串的场景多，这里只做演示
```
后面三个字符 substr slice substring 第一个是开始位置 区别在于第二个字符
slice substring 第二个是位置(不包含)
substr 第二个参数是个数，即返回字符的个数,没有第二个参数,意味着从第一个参数起,返回所有
```js
var str = 'biyuqi';
console.log(str.slice(0))   //biyuqi
console.log(str.slice(1,3)) //iy

console.log(str.substring(0))   //biyuqi
console.log(str.substring(1,3)) //iy

console.log(str.substr(0))   //biyuqi
console.log(str.substr(1,3)) //iyu 不包含3 数量3
```
### 字符串位置的方法
与数组方法类似,返回字符串的位置(如果没有找到该字符,则返回-1),区别在于indexOf从字符串开始向后搜索,lastIndex相反
* indexOf()
* lastIndexOf()

```js
var str = 'hello world';
console.log(str.indexOf('o')) //4
console.log(str.lastIndexOf('o')) //7
console.log(str.indexOf('m')) //-1

//判断重复字符
//a 测试变量
if(str.indexOf(a) === str.lastIndexOf(a)){
    console.log("没有重复字符")
}else{
    console.log("有重复字符")
}
```
### trim()方法
ECMAScipr5为所有字符定义了trim()方法,去除字符前后所有空格
```js
var str = ' hello,sorld ';
console.log(str.trim()) //'hello sorld'

// trim() 仅支持IE9+
//so 兼容方法
function trim(s){
    var reg = /(^\s*)|(\s*$)/g;
    return s.replace(reg,'')
}
```
### 字符串模式匹配方法
** match()接收一个参数,要么是一个正则表达式,要么是一个RegExp对象**
* match() //与正则exec()方法类似
* replace()

返回一个数组,数组第一项与整个是与整个模式配的字符串,之后的每一项(如果有)保存着与正则表达式中的捕获组匹配的字符串,没有匹配则返回null

```js
var string = 'biyui,sdf,sdfsdf'
var reg = /(.),/;
console.log(string.match(reg))
```
![][1]

全匹配时返回匹配数组,没有匹配则返回null
```js
var string = 'biyui,sdf,sdfsdf'
var reg = /(.),/g;
console.log(string.match(reg))
```
![][2]

** replace(),接收两个参数：第一个参数可以使RegExp对象或者一个字符串,第二个参数可以使一个字符串或者一个函数 **
```js
//字符替换
var text = 'cat,bat,sat,fat';
var res = text.replace('at','ond');
console.log(res) // cond bat sat fat

//全部替换
var text = 'cat,bat,sat,fat';
var res = text.replace(/at/g,'ond');
console.log(res) // cond bond,sond fond
```
```js
//第二个参数函数
//1.只有一个匹配项时，会接收三个参数：模式匹配的项，位置，原始串
//2.如果有多个捕获组，传递的参数一次的模式匹配的匹配项1、2、3;最后两个参数依然是 位置 原始串

//替换特殊字符
function htmlreplace(s){
    return s.replace(/[<>"&]/g,function(match,pos,text){
        switch(match){
            case "<":
                return "&lt;";
            case ">":
                return "&gt;";
            case "&":
                return "&amp";
            case "\"":
                return "&quot;";
        }
    });
}
console.log(htmlreplace("<p>test</p>"))
//&lt;p&gt;test&lt;/P&gt;
```

[1]: http://oj7j5nuxv.bkt.clouddn.com/match-v.png
[2]: http://oj7j5nuxv.bkt.clouddn.com/match-val.png
