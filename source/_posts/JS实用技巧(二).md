---
title: JS实用技巧(二)
date: 2016-12-26 21:13:12
tags: [javascript,"Js技巧"]
categories: Javascript
---
> ### 该系列文章主要记录自己平常所用，js一些技巧，作为知识的积累。

## 1.十六进制颜色值的随机生成(2种方法)
```javascript
function getColor(){
	var   arrStr = ["0","1","2","3","4","5","6","7","8","9","a","b","c","d"],
			strHas = "#",
			index;
			for(var i=0;i<6;i++){
				index = Math.floor(Math.random()*14);
				strHas += arrStr[index];
			}
	return strHas;
}
getColor();//随机颜色值

function getColor2(){
	return "#"+("000"+Math.floor(Math.random()*16777215).toString(16)).slice(-6);
}
getColor2()//随机颜色值
```
说明：
1、16777215为16进制的颜色ffffff转成10进制的数字
2、转成16进制不足6位的以0来补充(经测试会有不足6位的情况出现)

## 2.Event兼容
```javascript
//兼容IE和Firefox的event对象
btn.onclick() = function(){
	e = window.event || e;
	//some code
}

//兼容srcElement和target(事件源)
var et = e.srcElement || e.target;
console.log(et.tagName);

//封装getEventTarget函数
function getEventTarget(e){
	e.window.event || e;
	return e.srcElement || e.target;
}
```
## 3.阻止冒泡，封装stopPropagation函数
```javascript
function stopPropagation(e){
	e = window.event || e;
	if(document.all){
		cancelBubble = true;
	}else{
		e.stopPropagation();
	}
}
//or
function stopPropagation2(e){
	e = window.event || e;
	if(e.stopPropagation){
		e.stopPropagation();
	}else{
		cancelBubble = true;
	}
}
//demo
btn.onclick = function(e){
	stopPropagation(e);
}
```
document.all可以判断浏览器是否是IE;

## 4.添加事件监听 兼容
```javascript
/*
	添加事件监听 兼容
	addEvent(elem,type,fn,choose);
	elem 需要添加事件监听的元素
	type 事件类型 一律不加on
	fn 回调函数（要做的事情）
	choose 冒泡或者捕捉 默认为false
*/
function addEvent(elem,type,fn,choose){
	if(arguments.length < 3){
		return ;
	}
	//给choose赋初值
	choose = choose || false;
	//判断浏览器是否支持addEventListener
	if(elem.addEventListener){
		elem.addEventListener(type,fn,choose);
	}else {
		elem.attachEvent('on'+type , fn , choose);
	}
}
// demo:
var btn = document.querySelector("#btn");
on(btn,"click",function(){
    console.log(888);
});
```
## 5.类型判断函数
```javascript
var Tyle = (function(){
	var Type = {},
		list = ['String','Array','Object','Number','RegExp','Null'];
	for(var i=0;i<list.length;i++){
		(function(type){
			Type['is'+type] = function(obj){
				return Object.prototype.toString.call(obj) === '[object '+type+']'
			}
		})(list[i])
	}
	return Type
})()

console.log(Tyle.isArray([])) // true
console.log(Tyle.isNumber(''))	// false
console.log(Tyle.isString(''))	// true
console.log(Tyle.isObject({}))	// true
console.log(Tyle.isRegExp(/\s/))	// true
console.log(Tyle.isNull(''))	// false
```
## 6.设置透明度
```javascript
function setOpacity(elem,level){
	node = typeof node == "string" ? document.getElementById(node) : node;//检测传进来node的类型
	if(document.all){
		node.style.filter = 'alpha(opacity='+level+')';
	}else{
		node.style.opacity  = level/100;
	}
}
//test
setOpacity("box1",20);
setOpacity("box2",80);
```


  [1]: http://oiukswkar.bkt.clouddn.com/Js-jq.jpg
