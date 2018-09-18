---
title: Babel核心用法记要
date: 2016-12-13 18:55:56
tags: [javascript,Es6]
categories: Javascript
---
## 前言
最近公司项目的需要，开始接触闻名已久的ES6了，学习之前网上看了不少资料，各大浏览器厂商的支持程度不一，遂需要转码器来转换ES6为ES5代码，[Babel](https://babeljs.io/ "Babel")是一个广泛使用的转码器,安装学习过程中遇到一些问题，所以有了今天这篇文章，以备查阅。
## Babel一句话介绍
一个js编译器，报浏览器不支持js装换成支持的js。 
```javascript
//转码前
input.map(item => item + 1);
//转码后
input.map(function(item){
	return item + 1;
})
```
<!-- more -->
## 一、配置文件.babelrc
Babel的配置文件是.babelrc，存放在项目根目录下，使用Babel第一步就是配置这个文件。（**该文件需要自己创建，随便新建一个txt文档把它重命名为 ** .xxxx. ** 即会最终生成 .xxxx文件，创建.bablerc 则 重命名为 ** .babelrc. ）。  [百度网盘下载](http://pan.baidu.com/s/1qY2cHDq "百度网盘下载")
该文件用来设置转码规则和插件，基本格式如下：  
```javascript
{
	"presets": [],
	"plugins": []
}
```
**presets**设置转码规则，官方提供以下规则集，可根据需要进行安装。 
```javascript
# ES2015转码规则
$ npm install --save-dev babel-preset-es2015

#react转码规则
$ npm install --save-dev babel-preset-react
```
将规则加入.babelrc
```javascript
{
	"presets": [
		"es2015",
		"react"
	],
	"plugins": []
}
```
! 注意，以下所有Babel工具和模块的使用，都必须先写好.babelrc。
## 二、安装命令行转码babel-cli
>Babel提供babel-cli工具，用于命令行转码。
```javascript
$ npm install --global babel-cli
```
基本用法如下：
* #### 转码结果输出到标准输出（即 命令行里展现)
```javascript
$ babel demo.js
```
* #### 转码结果写入一个文件, --out-file 或 -o 参数指定输出文件
```javascript
$ babel demo.js --out-file complete.js
# 或者
$ babel demo.js -o complete.js
```
* #### 转译目录到目录
```javascript
$ babel src --out-dir dist
# 或者
babel src -d dist
```
* #### 监听文件变化
```javascript
$ babel -w src -d dist
```
##### 上面代码是在全局环境下，进行Babel转码。这意味着，如果项目要运行，全局环境必须有Babel

##参考：[Babel 用户手册](https://github.com/thejameskyle/babel-handbook/blob/master/translations/zh-Hans/user-handbook.md#toc-babel-cli "Babel 用户手册")