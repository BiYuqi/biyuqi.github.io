---
title: window命令行下输入javac提示不是内部或外部命令解决方法
date: 2017-02-20 20:53:28
tags: [window,java]
categories: 经验
---
> ** 下午用Cordova打包app时，提示未安装jdk，可是我明明安装jdk了，java -version也有版本提示;这就奇了怪，于是网上各种搜罗，于是有了这篇文章 **

<!-- more -->
### 前言
** 本文的前提是电脑中已经安装好java，配置好环境变量后出现的问题(某些情况下);没有安装的请自行安装 ** [JAVA-jdk][1]**
### 正文
找到 ** 高级系统设置>环境变量>系统变量>Path **
path路径里面添加jdk和jre的路径下面bin文件夹路径,格式：
![][2]
** 我的安装路径可能与大家不一样,可自行选择路径追加到Path后面记得有分号;**
;C:\Program Files (x86)\Java\jdk1.8.0_101\bin;C:\Program Files (x86)\Java\jre1.8.0_101\bin
### 原理
** 加入绝对路径可以无视JAVA_HOME,win会把Path路径下面所有可执行的文件都枚举出来 **

[1]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[2]: http://oiukswkar.bkt.clouddn.com/javac.png
