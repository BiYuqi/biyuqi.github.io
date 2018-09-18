---
title: 浅谈LocalStorage响应事件
date: 2017-02-21 17:13:12
tags: [javascript,localStorage]
categories: Javascript
---
> ** 前几天群里看到有朋友提了一个这样的需求；A页面看视频,又在B页面打开相同视频，B页面视频看完时，A页面状态自动改变 **


<!-- more -->
### 回顾
** localStorage API有哪些？**
* localStorage.setItem()该方法接受两个参数——要创建/修改的数据项的键，和对应的值
* localStorage.getItem()可以从存储中获取一个数据项。该方法接受数据项的键作为参数，并返回数据值
* localStorage.removeItem() 接受一个参数——你想要移除的数据项的键，然后会将对应的数据项从域名对应的存储对象中移除
* localStorage.clear() 不接受参数，只是简单地清空域名对应的整个存储对象

具体的实践先不写了,后续有时间再写...(会有时间么)
### StorageEvent 响应存储的变化
先看下MDN的介绍:
>无论何时,Storage对象发生变化时(即创建/更新/删除数据项时,重复设置相同的键值不会触发该事件，Storage.clear() 方法至多触发一次该事件),StorageEvent事件会触发.在同一个页面内发生的改变不会起作用——在相同域名下的其他页面(如一个新标签或iframe)发生的改变才会起作用.在其他域名下的页面不能访问相同的Storage对象。

** 根据mdn的描述，说明该方法只能在通域名下使用 **

写个样板DEMO
```javascript
window.addEventListener('storage',function(e){
    e.key;//改变的数据项的键
    e.oldValue;//改变前的旧值
    e.newValue;//改变后的新值
    e.url;//改变的存储对象所在的文档的 URL
    e.storageArea;//以及存储对象本身
})
```
** 该方法给予了我们很多想象力,像开头所说不同页面同域名下,所发生的的变化都可以通过该方法进行捕捉，进而更新相应数据 **
### 演示DEMO
[DEMO地址](http://loadingmore.com/demo/src/html/localstorage/page1.html)
### 参考
* [Web Storage API][1]
* [LocalStorage][2]



[1]: https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API
[2]: https://developer.mozilla.org/zh-CN/docs/Web/API/Storage
[3]: http://oiukswkar.bkt.clouddn.com/localstorage.jpg
