---
title: 浅谈jsvascript随机函数Math.random()
date: 2016-12-27 21:13:12
tags: [javascript]
categories: Javascript
---

<!-- more -->
## 背景
** Math.random() 方法可返回介于 0 ~ 1 之间的一个随机数。**
** 返回指定范围的随机数(m-n)之间)的公式：Math.random()*(n-m)+m；**

## 随机数
随机数是统计学领域的一个概念，对于游戏来说也是意义非凡（不玩游戏的我~ ~ ~），用好随机数，其实可以实现很多有意思的效果.

炒个栗子：抽奖大转盘程序，每当转盘停止转动的时候，指针恰好指向奖品图片的正中点，有人会觉得这不自然，不符合随机的规律。指向一个随机的位置可能更能让人容易接受。

学过 JavaScript 的人都知道，应用随机数很简单，只要一个 Math.random() 就可以获得一个大于等于 0 小于 1 的浮点数。从一个集合中随机选择对象时，使用浮点数离散化后的结果作为选择集的索引：
如下：
```javascript
var arr = [...];//很多组数据的集合
var ranIndex = parseInt( Math.random() * arr.length );
var result = arr[ranIndex];
```
## 产生符合指定概率模型的随机数
就像我们做一些特效一样，缓慢的节奏可以是动画更加动感，应用合适的概率分布也能让页面更具有表现力。而概率分布需要足够大的样本集才能体现效果，在** [codepen][2](因为还没有弄好图片，大家可以移步去这里一探究竟)**经常可以看到各种粒子效果（烟花，火焰，炫光）都离不开概率分布.

讲个比较贴近现实的栗子，平常开发过程中，抽奖需求应该用的蛮多的，抽奖活动中所有的奖品中，苹果电脑 和 移动电源中奖概率会一样么？为每一种奖品配置不同的概率是任何一款抽奖平台的基本功能。
举个例子：假设某次抽奖活动，奖品概率设置如下：
* 小米背包:0.01
* 旅行箱:0.03
* 移动电源:0.06
* 不中奖（也看作一种商品）:0.9

意思就是在10000次抽奖中，抽中100个移动电源，300个旅行箱，600个移动电源，9000次不中奖.
基本设计思路：设计一个returnRandom()函数，返回一个大于0小于1的浮点数，将0到1之间等分成四段，每一段对应一个奖品，根据函数的返回值来决定命中那个奖品；所以returnRandom()函数的返回值要满足如下：
* 返回值区间 [0,0.25],概率 0.01
* 返回值区间[0.25,0.5],概率0.03
* 返回值区间[0.5,0.75],概率0.06
* 返回值区间[0.75,1],概率0.9


```javascript
function  returnRandom(){
	var m = Math.random(),
	n = Math.random()/4;
	if(m < 0.01){
		return 0 + n;
	}
	if(m < 0.03){
		return 0.25 + n;
	}
	if(m < 0.06){
		return 0.5 + n;
	}
	if(m < 1){
		return 0.75 + n;
	}
}
```
使用的时候，可以用熟悉的调用方式:
```javascript
var arr = ["小米背包","旅行箱","移动电源","谢谢参与"];
var ranIndex = parseInt( returnRandom() * arr.length );
var result = arr[ranIndex];
```
具体实现：
用博客中之前写的数组去重函数来实现统计运算一百万次的概率
```javascript
var test = [];//一个新的临时数组
for(let i=0;i<1000000;i++){
	test.push(parseInt( returnRandom()*4 ))//储存运算十万次后得到的数据
}
function unique1(){
	var obj = {};
	var result = [];//用来接收测到的数据
	for(var i=0;i<test.length;i++){//遍历当前数组
		if(!obj[test[i]]){//如果没有当前项
			result[test[i]] = 1
		}else{
			result[test[i]] ++;
	}
}
```
数据导入echart3.0后得出如下图：
![Math.random()测试概率][3]
## 概率的可配置化

上一段代码演示了如何生成符合指定概率模型的随机浮点数，它的缺限在于模型的数据硬编码在函数体内，维护起来有困难。

如果设计一个工厂函数，根据输入的权重数据来动态创建随机函数，会给实际应用中带来很大便利。
后续在更.....



  [1]: http://oiukswkar.bkt.clouddn.com/mathran.jpg
  [2]: http://codepen.io/taobaofed/full/VeYZvL/?count=10000
  [3]: http://oiukswkar.bkt.clouddn.com/mathrandom.png
