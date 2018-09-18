---
title: Js函数的throttle(节流)和debounce(防抖)
date: 2017-03-02 13:00:12
tags: [javascript,'函数']
categories: Javascript
---
> ** 某些场景下，比如响应鼠标移动或者影响窗口大小的resize事件,scroll事件,导致频繁的触发DOM操作，资源加载等重行为，导致页面卡顿甚至浏览器崩溃;网上看到一些解决的方案，遂做记录 **

![sdf][3]
<!-- more -->
### 什么是debounce? 什么场景用? 怎么实现?
** 关键词查询**

解释这个函数前，先想想如下场景：网站我们的网站有个搜索框，用户输入文本我们会自动联想匹配一些结果呈现给给用户，我们可能首先想到的就是监听keypress事件,然后进行一步查询结果,该方法本身没有问题，但是如果用户哭诉输入一连串几十个字符，那就要瞬间触发几十次请求，无疑,我们不想要这样的结果，我们要的是用户停止输入的时候才去触发查询请求,这个时候函数防抖就可以帮到我们.

**电梯超时**

想象每天上班大厦底下的电梯。把电梯完成一次运送，类比为一次函数的执行和响应。如果电梯里有人进来，等待15秒。如果又人进来，15秒等待重新计时，直到15秒超时，开始运送。

**函数防抖**

就是让某个函数在上一次执行后，满足等待某个时间内不再触发此函数后再执行，而在这个等待时间内再次触发此函数，等待时间会重新计算。
* 如果用手指一直按住一个弹簧，它将不会弹起直到你松手为止。
* 也就是说当调用动作n毫秒后，才会执行该动作，
* 若在这n毫秒内又调用此动作则将重新计算执行时间。

**实现**
```javascript
/**
* 空闲控制 返回函数连续调用时，空闲时间必须大于或等于 delay,callback 才会执行
* @param delay   {number}    空闲时间，单位毫秒
* @param callback {function}  请求关联函数，实际应用需要调用的函数
* @return {function}    返回客户调用函数
*/
function debounce(delay,callback){
    var timer = null;
    return function(){
        var that = this,
            args = arguments;
        clearTimeout(timer);
        timer = setTimeout(function(){
            callback.apply(that,args);
        },delay)
    }
}
//demo
//keypress时间结束200毫秒后执行相关回调事件
$('.input').addEventListener('keypress',debounce(200,function(){
    //do somecode
},false))
```
### 什么是throttle? 什么场景用? 怎么实现?
**scroll事件**

网站开发中经常会遇到这样的需求就是滚动浏览器滚动条的时候，更新页面上的某些布局或者数据或者调用后台的某些接口,同样，如果不对scroll函数加以限制的话,我们滚动一次滚动条就会产生N多次的调用了,throttle跟debounce不同,不是在等待某个时间后去执行函数，二是每间隔某个时间去执行函数

**自助饮料机**

好比一台自动的饮料机，按拿铁按钮，在出饮料的过程中，不管按多少这个按钮，都不会连续出饮料，中间按钮的响应会被忽略，必须要等这一杯的容量全部出完之后，再按拿铁按钮才会出下一杯。

**函数节流**

每间隔某个时间去执行某函数，避免函数的过多执行，这个方式就叫函数节流。
* 形像的比喻是水龙头或机枪，你可以控制它的流量或频率。
* throttle 的关注点是连续的执行间隔时间。
* 也就是会说预先设定一个执行周期，当调用动作的时刻大于
* 等于执行周期则执行该动作，然后进入下一个新周期。

**实现**
```javascript
/**
* 频率控制 返回函数连续调用时，callback 执行频率限定为 次/delay  
* 不满足间隔时间,延迟times后必须执行一次
* @param delay  {number}    间隔时间，单位毫秒
* @param times  {number}    延迟时间，单位毫秒
* @param callback {function}  请求关联函数，实际应用需要调用的函数
* @return {function}    返回客户调用函数
*/
function throttle(delay,times,callback){
    var startTime = (new Date()).getTime(),
        timer = null;
    return function(){
        var currTime = (new Date()).getTime(),
            that = this,
            args = arguments;
        clearTimeout(timer);
        if(currTime - startTime >= delay){
            callback.apply(that,args);
            startTime = currTime;
        }else{
            timer = setTimeout(function(){
                callback.apply(that,args);
            },times)
        }
    }
}

```
```javascript
//Demo
//图片懒加,就是一个不断需要滚动条来判断距离的一个解决方案;
//通过scroll时间不断的判断,为了避免大量的scroll触发,
//我们可以用throttle对其进行节流操作
//仅仅做演示 html css均不提供，后文会给出demo
function lazyload(){
    var okSeeHeight = document.documentElement.clientHeight;//可视区域高度
    var curScrollTop = document.body.scrollTop || document.documentElement.scrollTop;//滚动条高度
    var imgNums = document.querySelectorAll('img'),
        len = imgNums.length,
        sn = 0;//初始化图片位置 避免每次都重新计算
    for(var i=sn; i<len; i++){
        //到达可视区域
        if(imgNums[i].offsetTop < okSeeHeight + curScrollTop){
            //替换scr
            imgNums[i].src = imgNums[i].getAttribute('data-src');
            sn = i + 1;
        }
    }
}
//调用即可
//可以大大减少scroll的触发量,而不影响体验
//500毫秒执行  不满足就强制500毫秒运行一次回调函数
window.addEventListener('scroll',throttle(500,500,lazyload))
```
### 使用场景
只要牵涉到连续事件或频率控制相关的应用都可以考虑到这两个函数，比如：

* 游戏射击，keydown 事件
* 文本输入、自动完成，keyup 事件
* 鼠标移动，mousemove 事件
* DOM 元素动态定位，window 对象的 resize 和 scroll 事件

前两者 debounce 和 throttle 都可以按需使用；后两者肯定是用 throttle 了。如果不做过滤处理，每秒种甚至会触发数十次相应的事件。尤其是 mousemove 事件，每移动一像素都可能触发一次事件。如果是在一个画布上做一个鼠标相关的应用，过滤事件处理是必须的，否则肯定会造成糟糕的体验。

### 图片懒加载演示DEMO
**[图片懒加载节流优化][4]**

**[github地址][1]**
### 参考
* [肥仔John][2]
* [高阶函数 debounce 和 throttle][5]



[1]: https://github.com/BiYuqi/lazyloading-img
[2]: http://www.cnblogs.com/fsjohnhuang/p/4147810.html
[3]: http://oiukswkar.bkt.clouddn.com/throttle.jpg
[4]: http://loadingmore.com/demo/src/html/lazyload.html
[5]: http://www.cnblogs.com/ambar/archive/2011/10/08/throttle-and-debounce.html
