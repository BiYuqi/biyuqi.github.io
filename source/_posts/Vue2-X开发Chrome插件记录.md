---
title: " Vue2.x开发Chrome插件记录"
subtitle: ""
date: 2017-06-04 21:37:58
author: "Yuqi Bi"
header-img: "form-opts.png"
cdn: 'header-on'
tags: ['Vue','chrome插件']
---
### 背景
作为一名web开发者,对[Animate.css](https://daneden.github.io/animate.css/)这个动画库不会陌生,它把常见的动画都封装了起来，非常实用。但是有时候在开发中，仅仅只是需要某一两个动画效果，把整个CSS文件都引入，这样不是太好。
<!-- more -->
需求就出现了，能不能有一个工具可以直接预览Animate.css对应的动画效果，并且生成对应的动画代码呢？

作为web开发者，平时跟Chrome浏览器打交道最多，于是就想着整一个Chrome插件可以及时预览对应Animate.css中的动画效果并生成对应的动画代码，这样在实际开发中碰到一些需要使用到Animate.css中的动画效果时，可以大大的提高我们的开发效率。

插件如下图所示：
![](http://oq4hkch8e.bkt.clouddn.com/prevcss.gif)
** [插件下载地址](http://pan.baidu.com/s/1bpozy8J) **
** [插件源码](https://github.com/BiYuqi/v-chrome) **
** [web网页版本](http://loadingmore.com/web-prev-animate/) **
### 开始
** Chrome插件开发基本知识 **
在应用商店中下载下来的插件基本上都是以.crx为文件后缀，该文件其实就是一个压缩包，包括插件所需要的html、css、javascript、图片资源等等文件。

开发一个插件就跟我们平时做web开发流程没多大的区别，就是先搭好基本的页面，然后使用js来写交互逻辑等功能。

比如我这个插件的目录文件如下：
![](http://oq4hkch8e.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170604220601.png)

** manifest.json文件 **
文件中需要注意一下的mainfest.json(很重要)这个文件，这个json文件的作用是提供插件的各种信息，例如插件能够做的事情，以及插件的文件配置等等信息。下面是一个清单文件的示例：
```js
{
    "manifest_version": 2,
    "name": "Prev CSS3 Animate",
    "description": "Preview CSS3 animation effects",
    "version": "1.0",
    "content_security_policy": "script-src 'self' 'unsafe-eval'; object-src 'self'",
    "permissions": [
        "http://*/*",
        "https://*/*",
        "file:///*"
     ],
    "content_scripts": [
        {
            "matches": [
                "http://*/*",
                "file:///*/*"
            ],
            "css": ["dist/style.css"]
        }
    ],
    "browser_action": {
        "default_icon": "img/icon4.png",
        "default_popup": "index.html"
    }
}
```
第一行声明我们使用清单文件格式的版本 2，必须包含（版本 1 是旧的，已弃用，不建议使用）

接下来的部分定义扩展程序的名称、描述与版本。这些都会在 Chrome 浏览器中使用，向用户显示已安装的扩展程序，同时在 Chrome 网上应用店中向潜在的新用户显示您的扩展程序。名称应该简练，描述不要比一句话左右还长

browser_action 对应的是默认的logo(本文是icon4.png)以及文件入口(文本为index.html),这两个资源都必须在扩展程序包中存在，图片是扩展的显示，html是扩展具体运行的基础文件。

### 功能实现
整个插件的核心交互功能非常简单，如文章开头的动图所示，用户选择对相应动画，代码区域显示对应的代码。这种简单数据交互使用vuejs再适合不过了，vuejs基础知识这里就不再细说了。可直接拷贝代码

这里需要注意的一点是，chrome 扩展的运行环境有一些特殊要求，称为 Content Security Policy (CSP)，使得通常的 vue 不能被正常使用。如果用的是 vue 1.x，那么可以下载 csp 版本,** [看这里](https://github.com/vuejs/vue/tree/csp/dist) ** 如果是 2.x 版本，请参考官网文档的 ** [这一段](https://vuefe.cn/v2/guide/installation.html#CSP-环境) **

其实2.x解决cps问题很简单，只需要在manifest.json里面加入这样一行即可随意使用2.x版本的Vue
```js
"content_security_policy": "script-src 'self' 'unsafe-eval'; object-src 'self'"
```
核心代码：
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <!-- 因为使用的webpack 所以直接饮用了打包后的文件 -->
        <link rel="stylesheet" href="dist/style.css">
    </head>
    <body>
        <div id="app-prev-css">
            <div class="header">Prev CSS3 Animate.css</div>
            <div class="prev-box">
                <div :class="[activeClass, errorClass]"></div>
            </div>
            <div class="select-line">
                <select v-model="selected">
                    <option v-for="(option,index) in options" :value="option">{{option}}</option>
                </select>
                <button type="button" @click="copyTips" class="copy-btn" data-clipboard-target="#copyCss">复制</button>
            </div>
            <div class="prev-css" v-html="cssText" id="copyCss" v-cloak>{{cssText}}</div>
            <div class="tips" v-show="isShow" v-cloak>复制成功！</div>
        </div>
        <!-- 因为使用的webpack 所以直接饮用了打包后的文件 -->
        <script src="dist/bundle.js"></script>
    </body>
</html>
```
CSS就不列出来了，可以在源代码中查看。

下面来使用vuejs来实现插件的功能。
```html
<select v-model="selected">
    <option v-for="(option,index) in options" :value="option">{{option}}</option>
</select>
```
使用 v-for 指令进行列表渲染。

用v-bind方法来绑定option的value值

在select中使用v-model方法来监听选中的值

```js
new Vue({
    el:"#app-prev-css",
    data () {
        return {
            selected:'bounce',
            activeClass: 'animated',
            errorClass: 'bounce',
            isShow:false,
            options:[
                'bounce','flash','pulse','rubberBand','shake','headShake','swing','tada','wobble','jello',
                'bounceIn','bounceInDown','bounceInLeft','bounceInRight','bounceInUp',
                'bounceOut','bounceOutDown','bounceOutLeft','bounceOutRight','bounceOutUp',
                'fadeIn','fadeInDown','fadeInDownBig','fadeInLeft','fadeInLeftBig','fadeInRight',
                'fadeInRightBig','fadeInUp','fadeInUpBig','fadeOut','fadeOutDown','fadeOutDownBig',
                'fadeOutLeft','fadeOutLeftBig','fadeOutRight','fadeOutRightBig','fadeOutUp','fadeOutUpBig',
                'flip','flipInX','flipInY','flipOutX','flipOutY','lightSpeedIn','lightSpeedOut',
                'rotateIn','rotateInDownLeft','rotateInDownRight','rotateInUpLeft','rotateInUpRight',
                'rotateOut','rotateOutDownLeft','rotateOutDownRight','rotateOutUpLeft','rotateOutUpRight',
                'slideInUp','slideInDown','slideInLeft','slideInRight','slideOutUp','slideOutDown','slideOutLeft',
                'slideOutRight','zoomIn','zoomInDown','zoomInLeft','zoomInRight','zoomInUp',
                'zoomOut','zoomOutDown','zoomOutLeft','zoomOutRight','zoomOutUp','hinge','rollIn','rollOut'
            ]
        }
    }
})
```
### 代码同步显示
接下来是代码同步功能，即在代码区域显示对应animate对齐的CSS代码。

这里我们用vuejs中的computed属性方法来返回数据
```js
computed:{
    cssText () {
        // 数据我单独放在json里面了 返回对应名字动画即可
        var  baseData = data[this.selected]

        // 处理数据 优化显示方式 换行 缩进代码 使用了showdown => html
        var converter =  new showdown.Converter({
          tables: true
        });
        baseData = converter.makeHtml(baseData);

        baseData = baseData.replace(/(.*?)gs/g,"<div>$1</div>")
        baseData = baseData.replace(/<p>|<\/p>/g,"")
        baseData = baseData.replace(/\b(sp)\b/g,"<span class='space'></span>")
        baseData = baseData.replace(/\b(sp2)\b/g,"<span class='space2'></span>")
        // 返回
        return baseData
    }
},
```

### 复制代码
使用了Clipboard插件,具体使用方法[github地址看这里](https://github.com/zenorocha/clipboard.js)
```html
<!-- 点击按钮绑定 data-clipboard-target="#copyCss" -->
<button type="button" @click="copyTips" class="copy-btn" data-clipboard-target="#copyCss">复制</button>

<!-- 代码区域使用 id copyCss-->
<div class="prev-css" v-html="cssText" id="copyCss" v-cloak>{{cssText}}</div>
```
接下来就是js
```js
mounted:function() {
    this.$nextTick(function(){
        new Clipboard('.copy-btn');
    })
}
```
开发好之后，可以直接在chrome中运行来调试。打开扩展面板，勾选开发者模式，然后加载刚开发扩展所在的目录就可以直接运行

一个简单的插件就完成了，通过这一个简单的chrome插件就可以体验到vuejs在web开发中简单、优雅的魅力，还有什么理由不用起来呢。

![](http://oq4hkch8e.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE201.png)

![](http://oq4hkch8e.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170604223202.png)
