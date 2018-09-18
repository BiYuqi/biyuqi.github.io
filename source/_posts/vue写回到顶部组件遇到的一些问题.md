---
title: 写Vue回到顶部组件遇到的一些问题
date: 2017-10-15 12:13:12
tags: [Vue,Javascript]
categories: Javascript
---

<!-- more -->
> 人的懒惰是有惯性的，写博客也是一旦停下来就会一直懒下去，相反一旦坚持下来，也会有惯性，所以我要开始更文啦...

## 效果
![](http://oq4hkch8e.bkt.clouddn.com/backTop.gif)
[组件地址](https://github.com/BiYuqi/daily-practice/blob/master/Vue/todo-app/src/components/BackTop/BackTop.vue)
## 问题

### 1.谷歌浏览器下document.body.scrollTop失效
经查阅此版本(版本为61)后的均需要使用document.documentElement.scrollTop来替代
### 2.vue怎么监听滚动事件
```html
<div class="back-top" @click="goTop" name="button" v-show="visible" :style="customStyle">
    <svg width="16" height="16" viewBox="0 0 17 17" xmlns="http://www.w3.org/2000/svg" class="back-icon" aria-hidden="true" style="height: 16px; width: 16px;">
        <g>
          <path d="M12.036 15.59c0 .55-.453.995-.997.995H5.032c-.55 0-.997-.445-.997-.996V8.584H1.03c-1.1 0-1.36-.633-.578-1.416L7.33.29c.39-.39 1.026-.385 1.412 0l6.878 6.88c.782.78.523 1.415-.58 1.415h-3.004v7.004z" fill-rule="evenodd"></path>
        </g>
      </svg>
</div>
```
```js
export default {
    props:{
        visibleHeight: { // 按钮出现条件
            type: Number,
            default: 400
        },
        rate: {
            type: Number, // 滚动速率
            default: 4
        },
        customStyle: { // 默认样式
            type: Object,
            default() {  // 此处由于是Object 所以要返回一个函数
                return {
                    right: '50px',
                    bottom: '50px',
                    width: '40px',
                    height: '40px',
                    'border-radius': '4px',
                    'line-height': '45px',
                    background: '#e7eaf1'
                }
            }
        }
    },
    data() {
        return {
            scrollTop: '',
            visible: false, // 是否显示
            interval: null  // 定时器

        }
    },
    mounted() {
        // Dom加载完毕时监听scroll事件
        window.addEventListener('scroll', this.handleScroll)
    },
    beforeDestroy() {
        window.removeEventListener('scroll', this.handleScroll)
            if (this.interval) {
                clearInterval(this.interval)
            }
    },
    methods:{
        handleScroll() {
            // 判断条件
            this.visible = window.pageYOffset > this.visibleHeight
            this.scrollTop = window.pageYOffset
        },
        goTop(e) {
            this.interval = setInterval(()=>{
                this.scrollTop = this.scrollTop + (-this.scrollTop)/this.rate
                window.scrollTo(0,this.scrollTop)
                if(this.scrollTop <= 0){
                    window.scrollTo(0,0)
                    clearInterval(this.interval)
                }
            },13)
        }
    }
}
```

### 3.window.scrollTo(x,y)是什么鬼
此方法为滚动到文档中的某个坐标，x 是文档中的横轴坐标。y 是文档中的纵轴坐标。
该函数实际上和 window.scroll是一样的
[具体请看MDM介绍](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/scrollTo)
### 4.组件使用方法
```js
//1. 先引入
import BackTop from '@/components/BackTop/BackTop.vue'

```
```html
<!-- 2. 页面使用方法 -->
<back-top :visibleHeight="50" :rate="6"></back-top>
```
## 缓动动画算法
之前看过张鑫旭大神写过这个方法，所以本文就直接拿来用了，具体算法是
![](http://oq4hkch8e.bkt.clouddn.com/js-animate.png)
```js
A = A + (B - A) / 2

我下一秒的位置 = 现在位置 + 现在和初始之间距离的一半
```


## 参考算法：
[张鑫旭-分享一个即插即用的私藏缓动动画JS小算法](http://www.zhangxinxu.com/wordpress/2017/01/share-a-animation-algorithm-js/)
