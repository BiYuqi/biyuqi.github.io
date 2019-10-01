---
title: JS实用技巧(三)
date: 2017-01-02 10:44:38
tags: [javascript,"Js技巧"]
categories: Javascript
---
> ### 该系列文章主要记录自己平常所用，js一些技巧，作为知识的积累。


<!-- more -->
## 1.获取或设置元素样式
```javascript
/**
	css 获取或者设置元素的样式
	css($('div'),'width','100px');
*/
function css(elem,styles,attr){
	if(arguments.length <= 1){
		return;
	}
	if(arguments.length ===2){
		if(elem.currentStyle){
			 return elem.currentStyle[styles];
		}
		return window.getComputedStyle(elem)[styles];
	}
	elem.style[styles] = attr;
}
```
## 2.给元素添加Class样式
```javascript
/**
	给元素添加某个class样式
	elem 元素
	cName 添加的样式
	支持多个class添加 字符串用空格隔开
*/
function addClass(elem,cName){
	var reg = new RegExp('(^|\\s)' + cName+ '(\\s|$)');
	if(!reg.test(elem.className)){
		elem.className += '  ' + cName;//有个空格
	}
}
```
## 3.元素是否有某个Class样式
```javascript
//返回值 true or false
function hasClass(elem,hClass){
	var reg = new RegExp('(^|\\s)' + hClass+ '(\\s|$)');
	return (reg.test(elem.className)) ? true:false;
}
```
## 4.去除元素的某个class样式
```javascript
/**
	去除元素的某个class样式
	elem 元素
	rName 去除的样式
	支持多个class去除 字符串用空格隔开
*/
//调用了上个函数hasClass
function removeClass(elem,rName){
	if(hasClass(elem,rName)){
		var reg = new RegExp('(\\s|^)'+rName+'(\\s|$)', 'g');
		elem.className = elem.className.replace(reg, ' ');//‘ ’空格
	}
}
```
## 5.找出数组中第二大数字
```javascript
function getNum(arr){
	var arr = arr.sort(function(a,b){
		return b-a;//从大到小排序
	})
	return arr[1];//取出第二个
}
var test = [1,2,41,8,5,90,45,3];
console.log(getNum(test));//45
```
## 6.传入任意数字，自动求和
```javascript
var num = 0;
function sum(){
	for(var i=0;i<arguments.length;i++){
		num += arguments[i];
	}
	return num;
}
console.log(sum(1,2,3));//6
console.log(sum(2016,1));//2017
```
## 7.传入任意数字，找出最大数字
```javascript
var num = 0;
function getMax(){
	for(var i=0;i<arguments.length;i++){
		if(num < arguments[i]){
			num = arguments[i];
		}
	}
	return num;
}
console.log(getMax(1,2,4,12,89,56,32,15));//89
console.log(getMax(9,5,7,8,3,1));//9
```


  [1]: http://oiukswkar.bkt.clouddn.com/Js-jq.jpg
