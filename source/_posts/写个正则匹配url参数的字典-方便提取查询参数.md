---
title: 写个正则匹配url参数的字典(方便提取查询参数)
date: 2017-04-04 22:38:00
tags: [javascript,'正则','RegExp','查询参数']
categories: Javascript
---
> 项目经常会用到提取地址栏的参数的问题，之前都是随用随写，这不趁着放假，总结了一下

<!-- more -->
### 我的版本
```javascript
function getUrlPart(url){
    url = url == null ? window.location.href : url;
    var str = url.substring(url.indexOf('?')+1);
    // 或者 var str = url.split('?')[1];也可以拿到?后面参数
    var reg = /([^?&=]+)=([^?&=]+)/gi;
    var m,res = {};
    while (m = reg.exec(str)){
        res[m[1]] = m[2];
    }
    return res;
}
```
### 来个高程三的版本
```javascript
function getQueryStringArgs(){
    //取得查询字符串并渠道开头问好
    var qs = (location.search.length > 0 ? location.search.substring(1) : ""),

    //保存数据对象
    args = {},

    //取得每一项
    items = qs.length ? qs.split("&") : [],
    item = null,
    name = null,
    value = null,

    //for循环中使用
    i = 0,
    len = items.length;

    for(i=0;i<len;i++){
        item = items[i].split("=");
        name = decodeURIComponent(item[0]);
        value = decodeURIComponent(item[1]);

        if(name.length){
            args[name] = value;
        }
    }
    return args;
}
```
