---
title: Js上传图片本地预览几种姿势
date: 2017-11-18 17:00:12
tags: [javascript,fileReader,createObjectURL]
categories: Javascript
---
> 本主要总结前端开发中图片上传预览的几个方法 ** FileReader ** 和 ** URL.createObjectURL() **

## FileReader

1.什么是filereader？

> FileReader 对象允许Web应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，使用 File 或 Blob 对象指定要读取的文件或数据。 ---MDN

看下怎么利用它来简单的实现一个图片预览
```html
<input type="file" id="input">
```
```js
/**
*   @param {dom} StringOrObject input元素
*   @param {fn} Function 回调函数 返回bs64 file
*/

const imagePreview = function(dom,fn){
    if(typeof dom === 'string'){
        dom = document.querySelector(dom)
    }
    const bindEv = function(){
        // 这里默认单图上传
        const file = this.files[0]
        const reader = new FileReader()
        reader.onload = function(ev){
            // 这里得到的是base64图片编码
            const bs64 = ev.target.result
            // 返回给回调函数
            fn && fn(bs64,file)
        }
        // 读取指定的file文件，读取完毕后，在onload事件里面 result属性将返回base64URL
        reader.readAsDataURL(file)
    }

    dom.addEventListener('change',bindEv,false)
}
```
```js
imagePreview('#input',function(src,file){
    // src 就是base64图片 可进行预览操作
    //.file 为文件 可在此处进行上传操作
    const img = document.createElement('img')
    img.src = src
    document.body.appendChild(img)
    console.log(src)
    console.log(file)
})
```
** readAsDataURL: **
> 开始读取指定的Blob对象或File对象中的内容. 当读取操作完成时,readyState属性的值会成为DONE,如果设置了onloadend事件处理程序,则调用之.同时,result属性中将包含一个data: URL格式的字符串以表示所读取文件的内容.

## FileReader的问题
* 兼容性
* 一些安卓5.0系统以下的bug

** 兼容后面再解决，先着重解决下第二个问题 **
![](http://oq4hkch8e.bkt.clouddn.com/bs64-filereader-bug-fix.png)
```js
/**
*   正常的图片应该是
*   data:image/gif;data:image/png;;data:image/jpeg;data:image/x-icon;
*   而在Android的一些5.0系统以下(如4.0)的设备中,
*   有些返回的b64字符串缺少关键image/gif标识,所以需要手动加上
*/

const imagePreview = function(dom,fn){
    if(typeof dom === 'string'){
        dom = document.querySelector(dom)
    }
    // 增加一个类型判断函数
    const isType = function(name,type){
        return name.indexOf(type) > -1
    }
    const bindEv = function(){
        // 这里默认单图上传
        const file = this.files[0]
        const reader = new FileReader()
        reader.onload = function(ev){
            let bs64 = ev.target.result
            if(bs64 && bs64.indexOf('data:base64,') > -1){
                // 去除旧有的错误头部
                bs64 = bs64.replace(/data:base64,/,'')
                // 声明一个空字符用来保存下面判断的类型
                // 通过name后缀进行识别标注
                let typeFile = '',
                    name = file.name.toLowerCase();
                if(name && isType(name,'.png')){
                    typeFile = 'image/png;'
                }
                if(name && isType(name,'.jpg')){
                    typeFile = 'image/jpeg;'
                }
                if(name && isType(name,'.gif')){
                    typeFile = 'image/gif;'
                }
                if(name && isType(name,'.icon')){
                    typeFile = 'image/x-icon;'
                }
                bs64 = 'data:'+typeFile+'base64,'+bs64
            }
            fn && fn(bs64,file)
        }
        reader.readAsDataURL(file)
    }

    dom.addEventListener('change',bindEv,false)
}
```
Firefox | Chrome | Internet Explorer* | Opera | Safari
------ | ----- | -------| -------- |
3.6 |7 | 10 |未实现|未实现

下面我们来看下另一种方法
## URL.createObjectURL()
> 在每次调用 createObjectURL() 方法时，都会创建一个新的 URL对象，即使你已经用相同的对象作为参数创建过。当不再需要这些 URL 对象时，每个对象必须通过调用 URL.revokeObjectURL() 方法来释放。浏览器会在文档退出的时候自动释放它们，但是为了获得最佳性能和内存使用状况，你应该在安全的时机主动释放掉它们

```js
// 语法:
// 参数：blob
// 是用来创建 URL 的 File 对象或者 Blob 对象​
objectURL = URL.createObjectURL(blob);
```
```js
const imagePreview = function(dom,fn){
    if(typeof dom === 'string'){
        dom = document.querySelector(dom)
    }
    // 处理下兼容
    window.URL = window.URL || window.webkitURL
    const bindEv = function(){
        const file = this.files[0]
        const bs64 = window.URL.createObjectURL(file)
        const img = document.createElement("img")
        img.src = bs64
        img.onload = function(){
            window.URL.revokeObjectURL(this.src)
        }
        fn && fn(bs64, file)
    }
    dom.addEventListener('change',bindEv,false)
}
```
```js
imagePreview('#input',function(src,file){
    // src 就是blog对象 可进行预览操作
    // blob:null/6436b315-7d42-46e7-b447-a1a982048e61
    //.file 为文件 可在此处进行上传操作
    const img = document.createElement('img')
    img.src = src
    document.body.appendChild(img)
    console.log(src)
    console.log(file)
})
```
在现代浏览器中，支持度还是不错的：

Firefox | Chrome | Internet Explorer* | Opera | Safari(WebKit)
------ | ----- | -------| -------- |
3.6 |7 | 10 | 15	| 6 [1]
[1] 通过 webkitURL 前缀对象实现。


## 参考
1.[图片引用](https://www.cnblogs.com/saysmy/p/5626337.html)
2.[MDN-FileReader](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader)
3.[MDN-createObjectURL](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/createObjectURL)
