---
title: JS数组常用的方法总结
date: 2016-12-28 21:19:37
tags: [javascript]
categories: Javascript
---
## 前言
数组应该是js中比较常用到的数据类型了，它有着独有的数据特点：
* 数组可以保存任意的数据类型 (字符串，数值，对象)
* 大小动态调整(随着数据的添加自动增长)

<!-- more -->

## 创建方式
* Array构造函数
```javascript
var colors = new Array();
```
* 数组字面量表示
```javascript
var colors = ["red","blue","green"]
```
## 常用的方法
* ## push()
接收任意数量的参数，把他们逐个添加到数组末尾，并返回修改后的数组长度。
```javascript
var colors = [];
colors.push("red","green");
console.log(colors) //red green 添加两项

colors.push("black");
console.log(colors);// red green black 又追加一项
```
* ## pop()
从数组末尾移除最后一项，减少length值,返回移除的项.
```javascript
var colors = [];
colors.push("red","green","black");
var item = colors.pop();

console.log(item); //balck 返回抓取到的值

console.log(colors);//red green 原数组值减去

console.log(colors.length);//2
```
---
* shift()
移除数组中的第一个项，并返回该项，同时数组长度减1.
```javascript
var colors = [];
colors.push("red","green","black");
var item2 = colors.shift();

console.log(item2); //red 返回抓取到的值

console.log(colors);// green black原数组值减去

console.log(colors.length);//2
```
* unshift()
在数组前端添加**任意**个项，并返回新数组长度
```javascript
var colors = [];
colors.push("red","green","black");
var item3 = colors.unshift("yellow","limegreen");

console.log(item3); //red green 返回抓取到的值

console.log(colors);// green black black yellow limegreen原数组值减去

console.log(colors.length);//5
```
* ## sort() 排序方法
针对原数组进行的排序，不会生成新的数组
默认sort()不带参数是按照数组中的元素换成字符串进行比较(从小到大)
ex:
```javascript
 var arr = [1,21,8,2,7,5];
 arr.sort();// 1, 2, 21, 5, 7, 8
```
## 排序
```javascript
 var arr = [1,21,8,2,7,5];
 
//升序
arr.sort(function(a,b){
	return a-b;
})
console.log(arr);//1,2,5,7,8,21 真正意义上的从小到大

//降序
arr.sort(function(a,b){
	return b-a;
})
console.log(arr);//21,,8,7,5,2,1 真正意义上的大到小
```
## 数组操作方法
* ## concat(item1,item2)
基于当前数组中的所有项创建一个新的数组。不操作原数组.
```javascript
var arr1 = [1,2,3,4];
var arr2 = [1,7,8];
var con = arr1.concat(arr2);
console.log(con);//1,2,3,4,1,7,8
```
* ## slice 截取数组
基于当前数组中的一个或者多个项创建一个新的数组，接收一个或者两个参数，即**要返回项的起始和结束位置，如果只有一个参数，返回该参数指定位置开始到数组末尾的所有项，如果两个参数，返回起始和结束位置之间的项（但是不包括结束位置项）**.
```javascript
var arr1 = [1,2,3,4,5,6,7,8,9];
var arr2 = arr1.slice(1);
var arr3 = arr1.slice(1,4);

console.log(arr2);//2,3,4,5,6,7,8,9; 索引从0开始
console.log(arr3);//2,3,4；不包括最后一项

//如果slice 方法中有一个负数，则可以用数组长度加上该数来确定位置
```
* ## splice 
主要用途向数组中部插入项：操作的是原数组
### 删除：
可以删除任意数量项，只需制定2个参数：要删除的第第一项的位置和删除的项数。例如：splice(0,3);会删除数组中的前3项.
### 插入：
可以插入任意数量项，只需制定3个参数：起始位置，0(要删除的项数)，要插入的项。如果要插入多个，可再传第四，第五项...；ex:splice(2,0,"red","green"),从2位置开始插入“red”,"green";
### 替换：
可以插入任意数量项，只需制定3个参数：起始位置，0(要删除的项数)，要插入的项(不必与删除数量相同)。ex:splice(2,1,"red","green"),从2位置开始插入“red”,"green";
```javascript
var colors = ["red","green","blue"];
var removed = colors.splice(0,1);
console.log(colors);//green,blue
console.log(removed);//red 返回的数组中只包含一项

removed = colors.splice(1,0,"yellow","orange"); //从1位置插入2项
console.log(colors);//green yellow orange blue
console.log(removed);//返回空数组，因为没有删除项，仅仅插入

removed = colors.splice(1,1,"red","purple");
console.log(colors);//green red purple orange blue
console.log(removed);//ywllow
```
## 位置方法
>ES5为数组添加了两个位置方法：indexOf()   LastIndexOf().
都接收两个参数 : 要查找的项和(可选的)表示查找起点位置的索引;

* indexOf()
从数组的开头开始向后查找：
```javascript
var numbers = [1,2,3,4,5,6,7];
console.log(numbers.indexOf(3));//返回索引 2
```
* lastIndexOf()
从数组的开头开始向后查找：
```javascript
var numbers = [1,2,3,4,5,6,7];
console.log(numbers.lastIndexOf(3));//返回索引 4
```
注意：没有找到的情况下返回-1；比较数组每一项时，使用全等操作符；支持IE9+;  
## 迭代方法
ES5为数组定义了5个迭代方法：
* every():对数组中的每一项运行给定函数，如果该函数每一项都返回true，则返回true;
* filter():对数组中的每一项运行给定函数，返回该函数会返回true的项，组成的数组；
* forEach():对数组中的每一项给定函数，没有返回值；
* map():对数组中的每一项给定函数;返回每次函数调用的结果组成的数组；
* some():对数组中的每一项给定函数;如果该函数对任意一项返回true，则返回true;

** 以上方法中，最相似的是every some，都是用于查询数组中的项是否满足某个条件，every()，传入每一项都返回true时才为true，some()相反 **
```javascript
var numbers = [1,2,3,4,5,7,8];
var everyReault = numbers.every(function(item,index,array){
	return (item > 2);//
})
console.log(everyReault);//false 不满足条件
```
```javascript
var numbers = [1,2,3,4,5,7,8];
var everyReault = numbers.some(function(item,index,array){
	return (item > 2);//
})
console.log(everyReault);//true 满足条件
```
** filter() 自定函数确定是否在返回的数组中包含一项要返回一个大于2的数组**
```javascript
var num = [1,2,3,4,5,2,1];
var filterResult = num.filter(function(item,index,array){
	return (item >2)
})
console.log(filterReault); //[3,4,5,]
```
** map()函数也返回一个数组，该数组每一项都会在原始数组的对应项上运行传入函数的结果，例如给数组中每一项*2，返回乘积后的数组**
```javascript
var num = [2,3,4,5,6,7];
var mapResult = num.map(function(item,index,array){
	return (item*2)
})
console.log(mapResult);  //[4,6,10,12,14]]
```
** forEach() 对数组每一项进行遍历，没有返回值，本质上与使用for 循环迭代数组一样。**
```javascript
var num = [1,2,3,4,5];
num.forEach(functin(item,index,array){
	//执行some code
})
```
注：支持IE9+
## 归并方法
** ES5新增了两个归并数组的方法:reduce() 和 reduceRight()**
都会迭代所有数组，然后构建一个返回值，reduce()方法从数组第一项开始，遍历到最后，reduceRight()则相反。都接收两个参数：一个在每一项上调用的函数(可选)作为归并基础的初始值。
这两个方法接收4个参数：前一个值，当前值，项的索引，数组对象；
```javascript
//利用reduce方法可以执行求数组和
var num = [2,3,4,5];
var sum = num.reduce(function(prev,cur,index,array){
	return prev + cur;
})
console.log(sum); //14;
```
```javascript
//利用reduceRight方法可以执行求数组和,方向相反
var num = [2,3,4,5];
var sum = num.reduceRight(function(prev,cur,index,array){
	return prev + cur;
})
console.log(sum); //14;
```
注：支持IE9+
## 小结
