---
title: Javascript函数
date: 2016-12-12 20:18:51
tags: [javascript]
categories: Javascript
---
## 概念
函数是一块Javascript代码，被定义一次，但可执行和调用多次。  
JS中的函数也是对象，所以JS函数可以像其他对象那样操作和传递。  
所以我们也常叫JS中的函数为函数对象。  
例如：  
```javascript
function add(x,y){
	if(typeof x === 'number' && typeof y === 'number'){
		return x + y;
	}else{
		return 0;
	}
}
add(3,4); // 7
```
<!-- more -->
函数一般由3部分组成：  
* 函数名
* 参数列表
* 函数体  

## 调用方式
* 直接调用

```javascript
add();
```
* 对象方法

```javascript
o.add();
```
* 构造器  

```javascript
new Foo();
```
* call/applay/bind  

```javascript
fun.call(o);
```
## 函数声明与函数表达式

### 函数声明
就是对函数进行普通的声明  
```javascript
function add(a,b){
	return a + b;
}
```
### 函数表达式
* 将函数赋值给变量

```javascript
var add = function(a,b){
	// some code...
}
```
* 立即执行函数
把匿名函数用括号包裹起来，在直接调用。
  
```javascript
(function(){
	// some code..
})()
```
* 函数对象作为返回值

```javascript
return function(){
	// some code...
}
```
* 命名式函数表达式

```javascript
var add = function foo(a,b){
	//some code...
}
```
这种表达式还真的少见，怎么用？该用哪个名字呢？  
做一个测试：  
```javascript
var fun = function funcc() {};
console.log(fun === funcc);

//在IE6~8 得到false  
// 在IE9+,以及现代浏览器中 Uncaught RefererceReeor :funccc is not defined
```
那么命名函数表达式有什么作用呢？
* 一般用于调试方便，如果使用匿名函数，执行的时候看不到函数名，命名函数表达式是可以看到函数名的。  
* 或者在递归时，使用名字调用自己。

但是这两种用法都很不常见。了解即可。  

## 变量 & 函数的声明前置
例1，函数声明：
```javascript
var num = add(3,4);
console.log(num);

function add(a,b){
	return a＋ｂ；
}
```
例２，函数表达式：
```javascript
var num = add(2,4);

console.log(num);

var  add = function(a,b){
	return a+b;
}
```
案例1中得到的结果是7，而案例2中是：**Uncaugh TypeError :add is not a function.**  
因为函数和变量在声明的时候，会被前置到当前作用域的顶端。1中将函数声明*function add(a,b)*前置到作用域前端，2将声明**var add**前置到其作用域的前端了，并没有赋值。**赋值的过程是在函数执行到响应位置的时候才进行的。**
## Function 构造器
除了函数声明，函数表达式。还有一种创建函数对象的方式，是使用函数构造器。
```javascript
var foo = new Function('a','b','console.log(a+b);');
foo(3,4);//7

var foo2 = Function('a','b','console.log(a+b);');
foo2(3,4);//7
```
Function 中前面的参数为后面函数体的形参，最后一个参数为函数体。可以看到传入的都是字符串，这样的创建函数对象的方法是不安全的。

还有一点，Function 构造器的得到的函数对象，拿不到外层函数的变量，但是可以拿到全局变量。它的作用域与众不同，这也是很少使用的原因之一。
## 函数属性 && argumnets
### 函数属性 &arguments
```javascript
function foo(a,b,c){
	arguments.length; //2
	arguments[0]; //1
	arguments[0] = 10;
	a; //change to 10
	
	arguments[2] = 100;
	c; //still undefined
	arguments.callee === foo; //true
}

foo(4,5);
foo.length;//3
foo.name;//'foo'
```
* **foo.name 函数名**
* **foo.length 形参个数**
* **arguments.length 实参个数**

未传参数时，arguments[i]相应位置仍然是undefined。  
严格模式下，代码中的改变实参无效，a仍未1.同时callee属性失效.  
* 关于**callee**

callee 属性的初始值就是正被执行function对象。  
callee 属性是arguments对象的一个成员，他表示对函数对象本身的引用，
这有利于匿名函数的递归或者保证函数的封装性，例如用下面示例的递归求和。而该函数仅当相关函数正在执行时才可用。需要注意callee拥有length属相，这个属相有事用于验证还是可以的。  
arguments.length是实参长度。arguments.callee.length是形参长度，由此可以判断调用时形参长度是否和实参长度一致。
```javascript
function sum(num){
	if(num <=1){
		return 1;
	}else{
		return num + arguments.callee(num -1);
	}
}

sum(100); //5050 很熟悉的结果吧
```
* 参考：
>高程3