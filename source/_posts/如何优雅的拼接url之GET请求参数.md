---
title: 如何优雅的拼接url之GET请求参数
date: 2017-01-17 20:08:07
tags: [javascript]
categories: Javascript
---
>请求的URL后面带参数在项目中是很常见的，常用在的地方比如跳转到新页面或者搜索关键词等，刚好项目用到，就写了我认为比较简便的方法提取参数

<!-- more -->

### Demo
[点击查看demo][2]

[最新Utils源码ES6版](https://github.com/BiYuqi/js-utils/blob/master/src/core/query.js)
### 最常见的方式
```javascript
//无底洞
 url?arg1=value1&arg2=value2&arg3=value3...
```
### 实战开始
![enter description here][3]

> 图片服务挂了，会尽快补上

这是一个后台管理系统的退款订单查询页面，可以看到页面的关键词不下十个，选择里面很多状态，而且还要判断是否输入的状态，拼接起来那叫一个酸爽如果用if else 估计我会崩溃的.....
### 进入正题
重点在代码后半部分
```javascript
//html结构不在本文内讨论
function getUrl(){
    //点击查询， 点击事件里面拿到各个值
    $("#btn-search").on('click',function(){
    	//为了储存select的值，设的变量 无须在意，重点不在这里
        var pendingStatusUp,
              payStyle;

        var startValue = $(".start_datetime").val();
        var endValue = $(".end_datetime").val();
        var orderClass = $(".order-class").val();
        var pendingStatus = $(".pending-status").val();
        var orderNumber = $("#order-num-order").val();
        var personNumber = $("#order-person").val();
        var payStyle = $('.pay-style').val();

        // 问诊还是处方
         var orderClassUp = (orderClass == 0) ? '问诊' : '处方'

        //以上所有的值，根据自己的情况，拿到点击时每个框的值

        //重点 把所有的值存到一个对象里
        //重点 把所有的值存到一个对象里
        var obj = {
            startValue:startValue,
            endValue:endValue,
            orderClassUp:orderClassUp,
            pendingStatusUp:pendingStatusUp,
            orderNumber:orderNumber,
            personNumber:personNumber,
            payStyle:payStyle
        }

        //下面为核心函数部分，可独立封装为一个函数，
        //有数值对象传进去即可,具体可看DEMO
        //定义一个数组，储存接下来的参数
        var arr = [];
        //为length==0时加？
        if(arr.length == 0){
            arr.push("?");
        }
        //遍历obj
        for(i in obj){
            //只有obj里面的属性的值不为空时
            //过滤值为空的属性，保证正确传参数
            //进行push操作属性名 = 属性值 &
            //都会随着遍历正确的排列好等着您用
            if(obj[i] !== ''){
                arr.push(i);
                arr.push("=");
                arr.push(obj[i]);
                arr.push("&");
            }
        }
        //下面的操作为2步
        //首先数组变为字符串
        //最后一项会多个&符号，用正则去除
        var str = arr.join('').replace(/&$/,'');

        return str;
    })
}
```
浏览器打印得到的是：
```javascript
?orderClassUp=问诊&pendingStatusUp=全部&payStyle=全部
```
输入值再看：
```javascript
?orderClassUp=问诊&pendingStatusUp=全部&orderNumber=99999999&personNumber=张三&payStyle=全部
```
### 封装util
```js
/**
* ajax请求参数优雅拼接 返回 ？a=v1&b=v2 ....
* @param { object } 键值对
*   {
*       id:'123',
*       name:'小芳',
*       keyword:''
*   }
*/
var _requireParam = function(obj){
    var rangeArr = [],
        param = '';
    if(obj && typeof obj === 'object'){
        if(rangeArr.length == 0){
            rangeArr.push('?')
        }
        for(i in obj){
            if(obj.hasOwnProperty(i)){
                if(obj[i] !== ''){
                    rangeArr.push(i)
                    rangeArr.push('=')
                    rangeArr.push(obj[i])
                    rangeArr.push('&')
                }
            }
        }
        param = rangeArr.join('').replace(/&$/,'')
        return param
    }
}

// 测试
var testObj = {
    id:'123',
    name:'小芳',
    keyword:''
}
_requireParam(testObj)
// ?id=123&name=小芳   已经自动过滤掉为空的字段
```
最终需要用提供的接口加上拿到的参数
```javascript
url = url + str;
```

[1]: http://oiukswkar.bkt.clouddn.com/url.jpg
[2]: https://biyuqi.github.io/demo/src/html/url.html
[3]: http://oiukswkar.bkt.clouddn.com/search-url.png
