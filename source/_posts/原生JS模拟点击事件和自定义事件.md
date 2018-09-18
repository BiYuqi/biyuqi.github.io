---
title: 原生JS模拟点击事件和自定义事件
date: 2017-06-11 16:46:45
tags: [javascript]
author: "Yuqi Bi"
header-img: "form-opts.png"
cdn: 'header-on'
---
> 事件，就是网页中某个特别值得关注的瞬间，事件是经常有用户操作或通过其他浏览器功能来触发，但是很少有人知道，也可以使用javascript来在任意时刻触发特定事件，此时事件如同浏览器创建的事件一样。 ------ 《高级程序设计3》

## 高程三的实现方法
模拟点击事件应该是一个很常见的行为，经常用jQ来实现，今天研究下原生js如何模拟触发点击事件
```js
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
    </head>
    <body>
        <div id="container">
            点击
        </div>
        <script>
            var doms = document.querySelector('#container')
            doms.addEventListener('click',function(){
                console.log("莫名其妙被点击")
            })
            // 使用createEvent() 创建event事件 传入鼠标事件字符串 MouseEvents
            var events = document.createEvent("MouseEvents")
            // 返回的对象有一个名为 initMouseEvent()方法 用于指定与该鼠标售价仅有关的信息
            events.initMouseEvent("click",true,true,document.defaultView)
            // 最后一步触发事件 dispatchEvent()
            doms.dispatchEvent(events)
        </script>
    </body>
</html>
```
很遗憾的是，在我查过MDN后，得知该方法已经废弃
** 注：该特性已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用该特性。 **

## MDN MouseEvent()
MDN上已经说得很清楚,尽管为了保持向后兼容MouseEvent.initMouseEvent()仍然可用,但是呢,我们应该使用MouseEvent().
```js
语法：event = new MouseEvent(typeArg, mouseEventInit);
```
```js
var doms = document.querySelector('#container')
doms.addEventListener('click',function(){
    console.log("莫名其妙被点击")
})
// 创建模拟对象
var events = new MouseEvent("click",{
    bubbles:true,
    cancelable:true,
    view:window
})
// 最后一步触发事件 dispatchEvent()
doms.dispatchEvent(events)

// 至此 我们的自动触发事件已经完成 控制台已经打印：莫名其妙被点击
```
typeArg|DOMString 格式的事件名称。
------------ | ----------
mouseEventInit | 可选
更多的属性请看这里[点击查看](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/MouseEvent#浏览器兼容性)

## IE兼容
```js
var doms = document.getElementById('container');
// 注意低版本ie兼容
doms.attachEvent('onclick', function () {
    console.log("莫名其妙被点击")
});

// 创建事件对象
var event = document.createEventObject()
// 触发
doms.fireEvent('onclick',event)
```
先创建enent对象，然后为其制定相应的信息，然后触发事件
fireEvent()方法会自动为event对象添加scrElement和type属性，其他属性都必须手动添加

## 创建自定义事件
** 自定义事件有两种方法,一种是使用new Event(),另一种是new customEvent() **
```js
var doms = document.getElementById('container');
// 创建自定义事件
var event = new Event('build',{
      bubbles: 'true',
      cancelable: 'true'
});

// 监听事件
doms.addEventListener('build', function (e) {
    console.log("测试自定义事件")
}, false);

// 触发事件.
doms.dispatchEvent(event);
// 控制台
// 测试自定义事件
```

```js
var doms = document.getElementById('container');
// 创建自定义事件
var event = new CustomEvent('build',{
      bubbles: 'true', //一个布尔值,表明该事件是否会冒泡.
      cancelable: 'true', //一个布尔值,表明该事件是否可以被取消.
      detail:"我是测试数据" //当事件初始化时传递的数据.
});

// 监听事件
doms.addEventListener('build', function (e) {
    console.log("测试自定义事件")
    console.log(e.detail)
}, false);

// 触发事件.
doms.dispatchEvent(event);
// 控制台
// 测试自定义事件
```
可以很明显的看到,其实new customEvent()比new Event()多了可以在event.detail属性里携带自定义数据的功能

绝大多数现代浏览器中都会支持这个构造函数（Internet Explorer例外）。 要了解更为复杂的方法，可参考下面的 [过时的方法](https://developer.mozilla.org/zh-CN/docs/Web/Guide/Events/Creating_and_triggering_events)。
[customEvent()](https://developer.mozilla.org/zh-CN/docs/Web/API/CustomEvent)和[ Event()](https://developer.mozilla.org/en-US/docs/Web/API/Event/Event) MDN链接在此

总结下来发现,除了模拟自定义事件比较好用,兼容性还可以，模拟鼠标事件兼容性各方面都支持的不太好，DOM这块水很深啊，继续研究...
