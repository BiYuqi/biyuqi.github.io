---
title: 编写一个Vue clipboard指令-快速复制文本到剪贴板
date: 2018-06-03 19:20:07
tags: [Vue, '组件', '指令', 'directive']
categories: Javascript
---
> 记录下用vue指令来写一个快速复制内容到剪贴板的插件, 主要是使用了chrome66+提供的新的剪贴方法，clipboard方法

## 项目地址
[Vue-element-admin, 欢迎start](https://github.com/BiYuqi/vue-element-admin)
[directive/index.js](https://github.com/BiYuqi/vue-element-admin/blob/master/src/directive/clipboard/index.js)
[项目预览,图标模块使用](http://loadingmore.com/vue-element-admin-preview/)
## directive部分
由于项目使用的element-ui库，所以提示信息组件，我就直接应用了，如有需要或者去除，请自行修改
```js
import { Message } from 'element-ui'

const Clipboard = {}

// 创建一个全局文本框 针对非chrome浏览器，以及chrome浏览器版本小于66的兼容方法
const input = document.createElement('input')
input.id = 'byq-clipboard'
input.type = 'text'
input.style.position = 'absolute'
input.style.left = '-9999px'
document.body.appendChild(input)

const copyTarget = document.querySelector('#byq-clipboard')
// 浏览器以及相关验证
const UA = window.navigator.userAgent.toLowerCase()
const isEdge = UA && UA.indexOf('edge/') > 0
const isChrome = UA && /chrome\/\d+/.test(UA) && !isEdge
// 确认是chrome浏览器，且版本符合要求
const isSupportChromeVersion = (v) => {
  return isChrome && ~~UA.match(/chrome\/(\d+)/)[1] >= v
}
Clipboard.install = (Vue, options) => {
  Vue.directive('clipboard', {
    bind (el, binding) {
      // 注册事件
      el.addEventListener('click', () => {
        const value = binding.value
        // 必须传值
        if (!value) {
          Message.error('请输入要复制问的文本')
        }
        // chrome version 66+ support
        if (isSupportChromeVersion(66) && window.navigator.clipboard) {
          window.navigator.clipboard.writeText(value).then(() => {
            Message.success('复制成功啦, 赶快使用吧')
          }).catch((error) => {
            Message.error(error)
          })
        } else {
          copyTarget.value = value
          copyTarget.select()
          document.execCommand('Copy')
          Message.success('复制成功啦, 赶快使用吧')
        }
      })
    }
  })
}

export default Clipboard

```

## 全局注册使用
** 引入注册组件 **
```js
// main.js 项目入口文件
import Clipboard from '@/directive/clipboard/index'
Vue.use(Clipboard)
```
** 页面使用 **
```html
<!-- 只需要传入v-clipboard  带上参数就可以了-->
<div class="test" v-clipboard="我是一个前端开发者">
  我是一个前端开发者
</div>
```

## navigator.clipboard 是个什么鬼？
这是一个实验中的功能，Clipboard接口提供了一种读写操作系统剪贴板的方式。
* read()
从剪贴板读取数据（比如图片）。
* readText()
从操作系统读取文本。
* write()
写入数据（比如图片）至操作系统剪贴板。
* writeText()
写入文本至操作系统剪贴板。

## 资料
[Clipboard|MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Clipboard)
