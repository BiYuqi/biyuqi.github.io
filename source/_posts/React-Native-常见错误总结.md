---
title: React Native 常见错误总结
date: 2016-12-21 19:58:16
tags: [React,React-native]
categories: React
---
> 随着RN的不断深入学习，遇到的坑也越来越多，错误也千奇百怪，自己就本着自己遇到的，和网上查询，总结为一篇博文，以备查阅，本文会持续更新...

<!-- more -->
## <font color="#FF0000">错误1：Element type is invalid…: </font>
错误描述:
Element type is invalid: expected a String(for built-in components) or a class/function(for composite components) but got:object. check the render method of ‘….’
<font color="limegreen">错误分析： </font>
这个错误是很不容易发现的原因是由于ES5语法和ES6语法混乱搭配导致的。比如用ES6 导出 用ES5导入, 就可能会产生上述错误.
- - -
## <font color="#FF0000">错误2：cannot call a class as a function </font>
<font color="limegreen">错误分析： </font>
这种错误一般都是手误导致的, 错误直译就是不能调用一个类作为一个函数。比如组件的单词写错。
- - -
## <font color="#FF0000">错误3： invariant violation:expected a component class,got[object object] </font>
<font color="limegreen">错误分析： </font>
创建自定义组件首字母要大写，否则会报错.
- - -
## <font color="#FF0000">错误4：Module 0 is not a registered callable module.</font>
<font color="limegreen">错误分析： </font>
 将gradle升级成最新版本(cdAndroid 进入android目录执行:sudo./gradlew clean) 或者通过android studio工具升级.
- - -
## <font color="#FF0000">错误5：Element type is invalid: expected a string (for built-in components) or a class/function but got: object</font>
<font color="limegreen">错误分析： </font>
 发生原因一般是你引用了无效的组件，如果组件确实正确，看下引用的组件是否正常导出：（export defalut）
- - -
## <font color="#FF0000">错误6：react native  undefined is not an object (evaluating this....</font>
<font color="limegreen">错误分析： </font>
  发生该错误的一般是忘记bind(this),只要回调函数中需要用到this的，一般都需要bind.
- - -
## <font color="#FF0000">错误7：react native - expected a component class, got [object Object]</font>
<font color="limegreen">错误分析： </font>
该错误可能是你引用了小写的组件，组件首字母一定要大写，比如<login/>应该写成<Login/>
- - -
## <font color="#FF0000">错误8：使用ListView时报错：Sticky header index 0 was outside the range {...}</font>
<font color="limegreen">错误分析： </font>
看起来是个数组越界错误，但多数情况下是由于ListView的子组件渲染错误（如套数据时没有检查undefined等）引起，而非ListView本身的问题。
- - -
## <font color="#FF0000">错误9：使用Image时报错：You are trying to render the global Image variable as a React element. You probably forgot to require Image.</font>
<font color="limegreen">错误分析： </font>
由于React的Image组件和全局的Image对象重名，所以使用Image组件时一定要记得在文件开头正确引入React的Image组件。
- - -
## <font color="#FF0000">错误10：在使用Navigator的同时使用ListView或ScrollView，后两者的头部会多出一些空间</font>
<font color="limegreen">错误分析： </font>
将automaticallyAdjustContentInsets属性设为{false}.
- - -
## <font color="#FF0000">错误11：报错：Adjacent JSX elements must be wrapped in an enclosing tag.</font>
<font color="limegreen">错误分析： </font>
render方法中必须只能包含一个根元素。
- - -
## <font color="#FF0000">错误12：报错：Invariant Violation: onlyChild must be passed a children with exactly one child</font>
<font color="limegreen">错误分析： </font>
一般是Touchable开头的几个组件，如果没有子元素或者指定多个并列子元素都会报错。
- - -
## <font color="#FF0000">错误13：报错：Invariant Violation: onlyChild must be passed a children with exactly one child</font>
<font color="limegreen">错误分析： </font>
一般是Touchable开头的几个组件，如果没有子元素或者指定多个并列子元素都会报错。
- - -
## <font color="#FF0000">错误14：报错：null is not an object (evaluating 'this.state.xxx')</font>
<font color="limegreen">错误分析： </font>
ES6的初始化需要把初始化的对象放在Constructor方法中，而不是放在getInitialState中；而如果是采用的是React.createClass的话就是可以把初始化放在getInitialState之中
- - -
## <font color="#FF0000">错误15：React Native(RN)启动不成功,unable to download js bundle错误解决方案</font>
<font color="limegreen">错误分析： </font>
.首先需要设置IP和端口，默认端口是8081，手机(模拟器)和电脑在同一个网络中，查询电脑的IP地址  
.最后重点  Android项目关闭终端重新执行运行命令，iOS项目也需要关闭服务终端,重新启动packager
- - -
## <font color="#FF0000">错误16：WebStorm不识别React Native语法解决方案</font>
<font color="limegreen">解决办法： </font>
遇到这个问题还是比较简单，只需要开启JSX语法支持即可，具体解决方案如下:
Preferences -> Languages and Frameworks -> JavaScript -> language version下拉框里选JSX Harmony
- - -
## <font color="#FF0000">错误17：com.android.ddmlib.InstallException: Unable to upload some APKs解决方案</font>
魅族 Meizu m2 note / 魅族 Meizu MX4 / 华为 Huawei Mate 7 / 华为 Huawei P8 / 小米 Redmi Note 2 / 乐视 Letv X500 无法安装以上手机安装apk时, 可能会报一个 com.android.ddmlib.InstallException: Unable to upload some APKs,
<font color="limegreen">解决办法： </font>
 我们需要修改如下几个位置:  

需要将 android/build.gradle 里的 gradle:1.3.1 改为 gradle:1.2.3
需要将 android/gradle/wrapper/gradle-wrapper.properties里的 distributionUrl 版本改为gradle-2.2-all.zip
- - -
