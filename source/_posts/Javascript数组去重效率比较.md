---
title: Javascript数组去重
date: 2016-12-04 00:33:30
tags: [javascript]
categories: Javascript
---
#### 前言 ####
面试前端必须准备的一个问题：怎样去掉javascript的array的重复项。据我所知，百度、腾讯、盛大等都在面试里出过这个题目。 这个问题看起来简单，但是其实暗藏杀机。 考的不仅仅是实现这个功能，更能看出你对计算机程序执行的深入理解.
<!-- more -->
#### 实现方式 ####
##### 1.数组去重第一种方法 #####
```javascript
    Array.prototype.unique1 = function(){
        var test = [];//一个新的临时数组
        var obj = {};
        for(var i=0;i<this.length;i++){//遍历当前数组
            if(!obj[this[i]]){//如果没有当前项
                test.push(this[i]);//把当前数组的当前项push到临时数组里面
                obj[this[i]] = 1;
            }else{
                obj[this[i]]++;
            }
    }
    //这里只用到了test 所以只return test; obj可以计数又可以去重
    return test;
};
```
##### 2.数组去重第二种方法 #####
```javascript
    Array.prototype.unique2 = function(){
        var test = [];
        for(var i=0;i<this.length;i++){
            if(test.indexOf(this[i]) == -1){
                test.push(this[i]);
            }
        }
        return test;
};
```
#### 结语 ####
其中第2种用到了数组的indexOf方法。此方法的目的是寻找存入参数在数组中第一次出现的位置。很显然，js引擎在实现这个方法的时候会遍历数组直到找到目标为止。所以此函数会浪费掉很多时间。 而第1中方法用的是对象。把已经出现过的通过下标的形式存入一个object内。下标的引用要比用indexOf搜索数组快的多。
