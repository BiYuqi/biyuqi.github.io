---
title: 原生js实现类似于Jquery的ajax请求方式
date: 2017-01-16 22:18:17
tags: [javascript]
categories: Javascript
---
> 最近用数据这块挺多，随手写了个原生Js的ajax封装，调用方式与JQuery一样

<!-- more -->
## 封装
```javascript
/*
* @obj 传入入口的对象属性和方法
* @type  请求方式 GET 或者. 默认GET
* @url 请求链接API
* @boolType 是否异步 true or false
* @dataType 数据类型 默认JSON
* @success 请求数据成功后的回调
* @error 请求数据失败的回调
*/
function ajax(obj){
    //定义请求数据方式
    obj.type = obj.type || "GET";
    //创建实例
    if(window.XMLHttpRequest){
		xhr = new XMLHttpRequest();
	}else if(window.ActiveXObject){
		xhr = new ActiveXObject(Microsoft.XMLHTTP);
	}
    //发送请求
    xhr.open(obj.type,obj.url,obj.boolType);
    //触发事件后
    xhr.onload = function(){
        if(xhr.readyState === 4){
            if(xhr.status >=200 && xhr.status <300 || xhr.status === 304){
                //判断数据类型
                if(obj.dataType && obj.dataType.toLowerCase() === "json"){
                    //声明类型 按json解析
                    return obj.success(JSON.parse(xhr.responseText));
                }
                 //默认输出
                return obj.success(xhr.responseText);
            }
        }else{
            return obj.error(new Error(xhr.statusText));
        }
    };
    //触发数据失败的提示
    xhr.onerror = function(){
        return obj.error(new Error(xhr.statusText));
    }
    //判断数据get or post
    obj.type && obj.type.toLowerCase() === "post" ?
		xhr.send(obj.data) : xhr.send(null);
}
```
## 调用：
```javascript
ajax({
    url:"https://biyuqi.github.io/gnotes/data/bloger.json",//数据可用，无跨域
    type:"GET",
    boolType:true,
    dataType:"JSON",
    success:function(data){
        console.log(data);//可以在浏览器查看数据了
    },
    error:function(error){
        console.log(error);
    }
})

```
## 追加一个promise方式的ajax之Get请求
update at 2017-12-29
```js
const ajax_GET = function(param){
    return new Promise((resolve,reject) => {
        const xhr = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject("Microsoft.XMLHTTP")

        xhr.open("GET",param.url,true)

        xhr.send()

        xhr.onload  = function(){
            resolve(xhr.responseText)
        }
        xhr.onerror = function(error){
            reject(error)
        }
    })
}
ajax_GET({url:xxxxxxxxx})
    .then((data)=>{
        console.log(JSON.parse(data))
    })
    .catch((err)=>{
        console.log(err)
    })

```
POST请求一样的道理，这里就不加赘述了

[1]: http://oiukswkar.bkt.clouddn.com/ajax.jpg
