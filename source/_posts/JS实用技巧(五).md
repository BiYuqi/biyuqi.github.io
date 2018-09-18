---
title: JS实用技巧(五)
date: 2017-02-04 21:15:55
tags: [javascript,"Js技巧"]
categories: Javascript
---
> ### 该系列文章主要记录自己平常所用，js一些技巧，作为知识的积累。

<!-- more -->
## 实现数组原生翻转reverse()方法
```javascript
function reverse(arr){
    //方法1
    var s = JSON.stringify(arr),//序列化
        o = [],
        reg = /(\w)+/g;//提取规则
    while(match = reg.exec(s)){
        //循环提取匹配
        o.unshift(match[0]);
    }
    return o;
    //方法2
    var o = [];
    for(var i=0;i<arr.length;i++){
        o.unshift(arr[i]);
    }
    return o;
}
//test:
var arrSt = ["bi","yu","qi","to","be","an"];
var res = reverse(arrSt);
console.log(res);//["an", "be", "to", "qi", "yu", "bi"]
```
## 根据传进去的对象,返回所有key为id的value
```javascript
function areaIds(data){
    var str = JSON.stringify(data),
        // reg = /\"id\":([^,])/g,
        reg = /\"id\":(\d)/,
        arr = [];
    while(match = reg.exec(str)){
        arr.push(match[1]);
    }
    return arr;
}
//test
var dd = {
    id:1,
    items:[
        {id:2},
        {id:3,items:[
            {id:4},
            {id:5}
        ]}
    ]
}
var d = areaIds(dd);
console.log(d);//["1", "2", "3", "4", "5"]
```
## 根据第一个参数array返回number数量的数的和
```javascript
/*
* var values = [2,3,32,8,5]
* minSum(values, 2); // => 2+3 = 5
* maxSum(values, 3); // => 32+8+5 = 45
*
*/
var testArr = [2,3,32,8,5];

function minSum(val,num){
    var sum = 0;
    val.sort(function(a,b){
        return a - b;
    });
    for(let i=0;i<num;i++){
        sum += val[i];
    }
    return sum;
}
var mini = minSum(testArr,2);
console.log(mini);//5

function maxSum(val,num){
    var sum = 0;
    val.sort(function(a,b){
        return b - a;
    });
    for(let i=0;i<num;i++){
        sum += val[i];
    }
    return sum;
}
var maxi = maxSum(testArr,3);
console.log(maxi);//45

```
## 实现数组拷贝累加
```javascript
//[1,2,3,4,5].duplicator(); // [1,2,3,4,5,1,2,3,4,5]
Array.prototype.duplicator = function() {
    //第一种方法
    var temp = this;
    temp = temp.join(',')+','+temp.join(',');
    return temp.split(',');
    //第二种
    var o = this.slice();
    for(var i=0;i<this.length;i++){
        o.push(this[i]);
    }
    return o;
    //第三种 最简便
    return this.slice().concat(this);
}
console.log([1,2,3,4,5].duplicator());
//[1,2,3,4,5,1,2,3,4,5]
```
[1]: http://oiukswkar.bkt.clouddn.com/Js-jq.jpg
