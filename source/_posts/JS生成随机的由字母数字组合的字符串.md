---
title: JS生成随机的由字母数字组合的字符串
date: 2016-12-12 22:05:29
tags: [javascript]
categories: Javascript
---
## 前言
网上偶然看到一个问题，需要生成3~32位长度的字母数字随机组合字符串，另一个是生成43位随机字符串.
代码：
```javascript
/*
** getWorld 产生任意长度随机字母数字组合
** getFlag 是否任意长度 min 任意最小长度（固定位数） max-任意最大长度
*/

function getWord(getFlag,min,max){
	var str = "";
	var range = min;
	var arr = ['0', '1', '2', '3', '4', '5', '6', '7', '8',
				'9', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h',
				 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q',
				'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', 'A',
				 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K',
				 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U',
				 'V', 'W', 'X', 'Y', 'Z'];
	if(getFlag){
		range = parseInt(Math.random() * (max - min)) + min;
	}
	for(var i=0;i<range;i++){
		var index = parseInt(Math.random() * (arr.length - 1));
		str += arr[index];
	}
	return str;
}
```
## 使用方法：
* 生成3~32位随机数：getWord(true,3,32)
* 生成43位随机串: getWord(false,43);
