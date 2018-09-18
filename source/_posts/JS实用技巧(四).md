---
title: JS实用技巧(四)
date: 2017-01-08 20:51:49
tags: ['javascript',"Js技巧"]
subtitle: ""
author: "Yuqi Bi"
header-img: "form-opts.png"
cdn: 'header-on'
---
> ### 该系列文章主要记录自己平常所用，js一些技巧，作为知识的积累。

<!-- more -->
## 1.判断函数的类型
```javascript
//ex:判断一个数据是否是数组Object.prototype.toString.call(obj)返回一个字符串
//比如 Object.prototype.toString.call( [1,2,3] ) 总 是 返 回 "[object Array]"
//Object.prototype.toString.call( “str”)总是返回"[object String]"

var isString = function(obj){
	return Object.prototype.toString.call(obj) === '[object String]';
}
var isArray = function(obj){
	return Object.prototype.toString.call(obj) === '[object Array]';
}
var isNumber = function(obj){
	return Object.prototype.toString.call(ojb) === '[object Number]';
}
//调用
console.log( isString('abc') );//true
console.log( isArray([1,2,3]) );//true
```
改进
```javascript
//这些函数大部分实现都是相同的，
//不同的只是返回的字符串的类型，
//可以把字符串作为参数提前植入
var isType = function(type){
	return function(obj){
		return Object.prototype.toString.call(obj) === '[object '+type+']';
	}
};
var isString = isType('String');
var isArray = isType('Array');
var isNumber = isType('Number');

//调用
console.log( isArray([1,2]) );//true
console.log( isArray('str') );// false
```
## 2.防止连续点击造成事件Bug
(比如轮播按钮快速点击，表单重复多次提交，这里就需要判断两次点击事件间隔，来做判断)
```javascript
//原生JS
var testBtnt = document.querySelector('.btn');
//初始时间戳
var _thistime = Date.now();
testBtnt.onclick = function(){
	//比较两次点击之间的时间，如果点击过快，return
	if((Date.now() - _thistime) < 700){//事件为毫秒，需要自己设定
		return false;
	}else{//符合点击范围
		//时间保留到上次点击的地方
		_thistime = Date.now();
		//do some code
	}
}
```
## 3.判断对象的属性存在于实例中，还是原型中
```javascript
/*
*object:实例对象
*name:要查询的属性
*/
function hasPrototypeProperty(object,name){
	return  !object.hasOwnProperty(name) && (name in object);
}
//in 只要通过对象能访问到属性就返回true
//hasOwnproperty()只有属性存在于实例中才会返回true
```
**Test：**
```javascript
function Person(){
}
Person.prototype.name = "loading";
Person.prototype.age = 34;
var person = new Person();
console.log( hasPrototypeProperty(person,'name') );//true;
person.name = "caricai";
console.log( hasPrototypeProperty(person,'name') );//false
```
## 4.RGBA实现随机颜色
```javascript
function getColor(){
    var strColor = [
        Math.floor(Math.random() * 255),
        Math.floor(Math.random() * 255),
        Math.floor(Math.random() * 255)
    ];
    var strNum = strColor.join(',');
    var rgba = 'rgba('+strNum+')';
    return rgba;
}
console.log(getColor());得到随机值rgba();
```
## 5.找出字符串中最长的单词并输出其长度
```javascript
var string = "The over the; lazy dog, May with you wohennia shabiad.";
function findLongWord(str){
    //对, . ; 先做了过滤 用空格分开 转化成数组 成为单词
    var str = str.replace(/[,.;]*/g ,'').split(' ');
    //进行长度比较
    var len = 0;
    //储存最长的单词
    var s = '';
    for(var i=0;i<str.length;i++){
        if(len < str[i].length){
            len = str[i].length;
            s = str[i];
        }
    }
    //返回值
    return ('最长单词是:'+s +',长度为:'+s.length)
}
console.log(findLongWord(string));
```
## 6.判断是否为空对象
```javascript
function isEmpty(obj){
   for(var key in obj){
	   return false;
   }
   return true;
};
var o = {};
isEmpty(o);//true
o.name="biyuqi";
isEmpty(o);//false
```
  [1]: http://oiukswkar.bkt.clouddn.com/Js-jq.jpg
