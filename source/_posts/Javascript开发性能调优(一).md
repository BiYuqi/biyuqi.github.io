---
title: Javascript开发性能调优(一)
date: 2017-02-16 21:00:06
tags: [javascript,"性能","优化"]
categories: Javascript
---
> ** 为什么我们做出速度很慢的网站，给用户一个糟糕的体验？**


<!-- more -->
### 异步加载第三方内容
有人没有加载过这样的第三方内容吗？比如第三方统计，打开很多网站，f12都会看到这些内容问题在于，不管是用户端的还是服务器端的连接，都无法保证这些代码是正常有效的工作的。这些服务有可能临时dowan掉或者是被用户或者其公司的防火墙阻止.为了避免这些在页面加载时成为问题，或者更严重的是，阻塞了全部页面的加载，总是应该异步加载这些代码.

```javascript
var script = document.createElement('script'),
    scripts = document.getElementsByTagName('script')[0];

    script.async = true;
    script.src = url;
    scripts.parentNode.insertBefore(script,scripts);

```
### 缓存数组长度
循环无疑是和Javascript性能非常相关的一部分。试着优化循环的逻辑，从而让每次循环更加的高效。要做到这一点，方法之一是存储数组的长度，这样的话，在每次循环时都不用重新计算。
```javascript
var arr = new Array(1000),
    i,len;

//Bad code
for(i=0;i<arr.length;i++){
    //需要计算一千
}
//Good Code
for(i=0;len = arr.length;i<len;i++){
    //计算一次长度
}
```
> 注解: 虽然现代浏览器引擎会自动优化这个过程，但是不要忘记还有旧的浏览器

** 在迭代document.getElementsByTagName('a')等类似方法生成的HTML节点数组（NodeList）时，缓存数组长度尤为关键。这些集合通常被认为是“灵活的”，意思就是说，当他们所对应的元素发生变化时，他们会被自动更新。**
```javascript
var elements  = document.getElementsByTagName('a'),
    i,len;
for(i=0;len = elements.length;i<len;i++){
    //do some code
}
```
注意:返回一个包括所有给定标签名称的元素的HTML集合HTMLCollection。 整个文件结构都会被搜索，包括根节点。返回的 HTML集合是动态的, 意味着它可以自动更新自己来保持和 DOM 树的同步而不用再次调用 document.getElementsByTagName()

### 最小化重绘和回流
当有任何属性或元素发生改变时，都会引起DOM元素的重绘和回流。当一个元素的布局不变，外观发生改变时，就会引起重绘。就像样式的改变，例如改变background-color。回流的代价是最高的，当改变一个页面的布局时就会发生回流。
```javascript
var item = document.getElementById("test"),
    lis = document.getElementsByTagName('li'),
    i, len;

for (i = 0, len = lis.length; i < len; i++) {
    //item.offsetWidth 会不停的计算item的宽度
    lis[i].style.width = item.offsetWidth + 'px';
}
```
改为
```javascript
var item = document.getElementById("test"),
    lis = document.getElementsByTagName('li'),
    itemWidth = item.offsetWidth,//缓存宽度
    i, len;

for (i = 0, len = lis.length; i < len; i++) {
    lis[i].style.width = itemWidth + 'px';
}
```
参考:
    [MDN][2]

[1]: http://oiukswkar.bkt.clouddn.com/optmini.jpg
[2]: https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementsByTagName
