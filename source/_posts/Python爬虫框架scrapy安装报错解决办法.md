---
title: Python爬虫框架scrapy安装报错解决办法
date: 2017-04-12 21:16:23
tags: [python,'scrapy']
categories: Python
---
> 最近尝试入门python,接触这门语言的目的很明确,就是爬虫,用来爬取数据,我对数据一直抱有神秘感，很感兴趣，所以闲暇之余,开始入门python,它大名鼎鼎的爬虫框架scrapy自然躲不过我的搜索,所以安装测试,于是便开始了这篇踩坑文章

<!-- more -->
### 报错原因
![][1]
我的报错原因就是如图中所示: ** error: Unable to find vcvarsall.bat**
网上也收了很多解决办法,但都是不但可行，对于初学者我来学还是蛮吃力,最终看到有篇帖子说是因为缺少c++一些环境变量引起的,安装Visual Studio 2015可以结局问题,所以网上找了个简版安装包,已经共享在了百度云盘:
** [我是下载链接][2]**

### 安装步骤
勾选如下几项即可
![][3]

安装完毕后关闭即可;

这个时候在命令行里:
** pip install scrapy **
这个时候基本上会顺利的安装完毕了
[1]: http://oiukswkar.bkt.clouddn.com/scrapy-error.png
[2]: http://pan.baidu.com/s/1pLTpCTl
[3]: http://oiukswkar.bkt.clouddn.com/visul.png
