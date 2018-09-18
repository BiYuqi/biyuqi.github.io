---
title: Js模仿块级作用域与ES6块级作用域
date: 2016-12-22 23:13:49
tags: [javascript]
categories: Javascript
---
>最近接触ES6多了起来，感觉自己之前js的基础还没有多牢固，趁着下班时间，着手温习下高程3，今天看到块级作用域这块，索性就把刚学一点的ES6做个比较，看异同，现学现卖了。

## 前传
ES5只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。
<!-- more -->
## 为什么需要块级作用域（ES5实现方式）？ 
**第一种场景，用来计数的循环变量泄露为全局变量。**
```javascript
function outputNums(count){
	for(var i=0;i<count;i++){
		console.log(i);// 0 1 2 3 4 5 6
	}
	console.log(i); //7
}
outputNums(7)
```
该函数定义了for循环，变量i初始值为0，变量i只用来控制循环，但是循环结束后，它并没有消失，泄露成了全局变量。在Javascript中，变量i是定义在outputNums()的活动对象中，因此函数内部可以随处访问。  
* 解决方法：匿名函数可以用来模仿块级作用域
```javascript
(function(){
	//这里是块级作用域
})()

以上立即执行函数相当于
var func = function(){
	//这里是块级作用域
};
func();
```
定义一个匿名函数，立即调用它，调用方式是在函数名字后面添加一对圆括号，即**func()**;  
如果省去变量直接加()呢？  
```javascript
function(){
	//这里是块级作用域
}()；//报错
```
因为JavaScript将function关键字当做一个函数声明的开始，而声明后面不能跟圆括号，然而表达式后面可以跟圆括号，将函数声明转化为函数表达式，只需如下：
```javascript
(function(){
	//这里是块级作用域
})()；
```
改写后：
```javascript
function outputNums(count){
	(function(){
		for(var i=0;i<count;i++){
			console.log(i);// 0 1 2 3 4 5 6
		}
	})()；
	console.log(i); //报错
}
outputNums(7)
```
我们在for循环外面加了一个私有作用域，在匿名函数定义的任何变量，都会在执行结束时销毁，因此i只能在循环中使用，使用后即被销毁，在私有作用域中可以访问变量count，是因为匿名函数是一个闭包，可以访问包含作用域中的所有变量。
举个例子：
```javascript
(function(){
	var nowDate = new Date();
	if(nowDate.getMonth == 0 && nowDate.getDate == 1){
		alert("新年快乐！");
	}
})()
```
上面这段代码，会在阳历一月一日，弹出新年祝福！在多人开发团队合作中，通过创建私有作用域，每个开发人员既可以使用自己的变量，也不用担心弄乱全局作用域。

**第二种场景，内层变量可能会覆盖外层变量。**
```javascript
var nowDate = new Date();
function func(){
	console.log(nowDate);
	if(false){
		var  nowDate = "新年快乐";
	}
}
func()//undefined
```
导致导致上面的结果原因是变量提升，导致内层nowDate覆盖外层nowDate；
错误还原如下：
```javascript
var nowDate = new Date();
function func(){
	var nowDate;
	console.log(nowDate);
	if(false){
		nowDate = "新年快乐";
	}
}
func()//undefined
```
## ES6的块级作用域 

**let实际上为javascript增加了块级作用域**
```javascript
function func(){
	let n = 5;
	if(true){
		let n = 10;
	}
	console.log(n);//5
}
```
上面两段代码都声明了n，运行后输出5，这表明外层代码不收内层代码的影响，如果用var 定义变量n，最后结果会输出10.
- ES6允许块级作用域的任意嵌套。
```javascript
{{{{ let n = "新年快乐" }}}}
```
下面这段代码使用了四层的会计作用域，外层的作用域无法读取内层作用域。
```javascript
{{{
	{ let n = "新年快乐" }
	console.log(n);//报错
}}}
```
内层作用域可以定义外层作用域的同名变量。
```javascript
{{{
	let n = "我是n"
	{ let n = "我也可以是n" }
	console.log(n);//我是n
}}}
```
## 展望下未来
**块级作用域的出现，实际上使得获得广泛应用的立即执行函数表达式（IIFE）不再必要了。**
```javascript
// IIFE 写法
(function () {
  var nowDate = "新年快乐";
  //...
})();

// 块级作用域写法
{
  let nowDate ="新年快乐";
  //...
}
```
结语：具体怎么用，还是得看支持的程度。 
共勉！