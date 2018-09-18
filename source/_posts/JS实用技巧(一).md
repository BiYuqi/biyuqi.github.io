title: JS实用技巧(一)
date: 2016-12-13 09:07:03
tags: [javascript,"Js技巧"]
categories: Javascript
---
> 该系列文章主要记录自己平常所用，js一些技巧，作为知识的积累。


<!-- more -->
## 1.用户判断给定的对象是否是数组
```javascript
function isArray(obj){
	return Object.prototype.toString.call(obj) === '[object Array]'；
}
// test
isArray('str'); //false
isArray([1,2]); //true
```

## 2.判断检查数组中是否存在某个值
```javascript
Array.prototype.inArray = function(a){
	for(var i=0;i<this.length;i++){
		if(this[i] == a){
			return true;
		}
	}
	return false;
}
//demo
[3,4].inArray(4);//true
```
## 3.用来显示或隐藏一个DOM元素
```javascript
function toggle(obj){
	var elem = document.querySelector(obj);
	if(elem.style.display != 'none'){
		elem.style.display = 'none';
	}else{
		elem.style.dispaly = '';//注意不要有空格
	}
}
//demo
oDiv.onclick = function(){//oDiv是点击元素
      toggle("obj");
}
```
> querySelector() 方法返回文档中匹配指定 CSS 选择器的一个元素。
注意： querySelector() 方法仅仅返回匹配指定选择器的第一个元素。如果你需要返回所有的元素，请使用 querySelectorAll() 方法替代。

## 4.加载样式文件
```javascript
function loadStyle(url){
	try{
		document.createStyleSheet(url);//ie
	}catch(err){
		var cssLink = document.createElement('link');
		cssLink.rel = 'styleSheet';
		cssLink.type = 'text/css';
		cssLink.href = url;
		var head = document.getElementsByTagName('head')[0];
		head.appendChild(cssLink);
	}
}
//test
loadStyle('css/style.css')；
```
## 5.获取时间的某部分
```javascript
var myDate = new Data();

//获取当前年份(2位)
myDate.getYear();
//获取完整的年份(4位，ex:2016)
myDate.getFullYear();
//获取当前月份(0~11)
myDate.getMonth();
//获取当日(1~31)
myDate.getDate();
//获取当前星期几(0~6)
myDate.getDay();
//获取当前时间戳
myDate.getTime();
//获取当前小时(0~23)
myDate.getHours();
//获取当前分钟数(0~59)
myDate.getMinutes();
//获取当前秒数（0~59）
myDate.getSeconds();
//获取当前毫秒数(0~999)
myDate.getMilliseconds();

myDate.toLocaleDateString(); //获取当前日期
myDate.toLocaleTimeString(); //获取当前时间
myDate.toLocaleString( ); //获取日期与时间

//时间戳
Date.now();

兼容
+Date.now();
//ie9+

```
## 6.元素显示的通用方法
```javascript
function $(id){
	 return !id ? null : document.getElementById(id);
}
function display(id) {
    var obj = $(id);
    if(obj.style.visibility) {
        obj.style.visibility = obj.style.visibility == 'visible' ? 'hidden' : 'visible';
    } else {
        obj.style.display = obj.style.display == '' ? 'none' : '';
    }
}
```


  [1]: http://oiukswkar.bkt.clouddn.com/Js-jq.jpg
