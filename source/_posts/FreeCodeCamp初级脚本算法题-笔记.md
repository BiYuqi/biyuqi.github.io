---
title: FreeCodeCamp初级脚本算法题-笔记
date: 2017-06-12 00:03:36
tags: [javascript,'算法']
author: "Yuqi Bi"
header-img: "form-opts.png"
cdn: 'header-on'
---
> [FreeCodeCamp](http://www.freecodecamp.cn/)是一个学习编程的开源社区,里面有面向前端方面的基础知识，也有一些脚本算法相关的题目，今天花了2个小时把初级脚本算法题16道，研究了一下(我刚做了15,还有一道需要研究下...后面补上+注释),通过几个小时的学习，领会到FCC是在实践练习中掌握知识，而不是学习背记理论了解知识,前端本身也是一种技术，所以多实践，多研究，才能加深对这门语言的理解.

## 翻转字符串算法挑战
```js
const reverseString = function (str) {
    return str.split('').reverse().join('')
}
```
## 阶乘算法挑战
```js
const factorialize = function (num) {
    let res = 1;
    for(let i=num;i>0;i--){
        res *= i
    }
    return res
}
```
```js
// 递归
const factorialize2 = function (num) {
    if(num <= 1){
        return 1
    }
    return num * factorialize2(num-1)
}
```
## 回文算法挑战
如果一个字符串忽略标点符号、大小写和空格，正着读和反着读一模一样，那么这个字符串就是palindrome(回文)
```js
const palindrome = function (str) {
    str = str.replace(/[^0-9a-zA-Z]/gi,'').toLowerCase()
    const len = str.length
    for(let i=0,j=len-1;i<len;i++,j--){
        if(str[i] !== str[j]){
            return false
        }
    }
    return true
}
console.log(palindrome('mnnm'))
```
## 寻找最长的单词算法挑战
```js
const findLongestWord = function (word) {
    let words = word.split(' '),
        max = 0,
        len = words.length;
    for(let i=0;i<len;i++){
        if(words[i].length > max){
            max = words.length
        }
    }
    return max
}
console.log(findLongestWord("The quick brown fox jumped over the lazy dog"))
```
## 设置首字母大写算法挑战.
```js
const titleCase = function (w) {
    let word = w.toLowerCase().split(' '),
        len = word.length;
    for(let i=0;i<len;i++){
        let filter = word[i].charAt(0).toUpperCase()
        word[i] = word[i].replace(word[i].charAt(0),filter)
    }
    return word.join(' ')
}
console.log(titleCase('I"m a little tea pot'))
```
## 寻找数组中的最大值算法挑战
大数组中包含了4个小数组，分别找到每个小数组中的最大值，然后把它们串联起来，形成一个新数组。
```js
const largestOfFour = function (arr) {
    let len = arr.length,
        res = [];
    // 写个执行解析出每个数组大值得函数
    function getMax(s){
        let ss = s.sort(function(a,b){
            return b - a
        })
        return ss[0]
    }
    for(let i=0;i<len;i++){
        // 动态添加每个最大值
        res.push(getMax(arr[i]))
    }
    // 返回
    return res
}

var stt = [[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]
console.log(largestOfFour(stt))
// [5, 27, 39, 1001]
```
## 确认末尾字符算法挑战
检查一个字符串(str)是否以指定的字符串(target)结尾。
如果是，返回true;如果不是，返回false。
```js
const confirmEnding = function (str,target) {
    let reg = new RegExp(""+target+"$",'gi')
    if(reg.test(str)){
        return true
    }
    return false
}
console.log(confirmEnding("He has to give me a new name", "me"))
```
## 重复操作算法挑战
重复一个指定的字符串 num次，如果num是一个负数则返回一个空字符串。
```js
const repeat = function(str,num){
    let res = '';
    if(num < 0){
        return ''
    }
    for(let i=0;i<=num;i++){
        res += str
    }
    return res
}
console.log(repeat('ser',3))
// serserser
```
## 符串截取算法挑战
如果字符串的长度比指定的参数num长，则把多余的部分用...来表示。
如果字符串的长度比指定的参数num短 返回原字符串
切记，插入到字符串尾部的三个点号也会计入字符串的长度。
但是，如果指定的参数num小于或等于3，则添加的三个点号不会计入字符串的长度。
```js
const truncate = function(str,num){
    let ss = '',
        tips = '...';
    if(str.length > num){
        if(num > 3){
            return str.substr(0,(num-3))+tips
        }
        return str.substr(0,num)+tips
    }else{
        return str
    }
}
console.log(truncate("A-tisket a-tasket A green and yellow basket", 8))
// A-tis...
```
## 数组分割算法挑战
把一个数组arr按照指定的数组大小size分割成若干个数组块。
例如:chunk([1,2,3,4],2)=[[1,2],[3,4]];
chunk([1,2,3,4,5],2)=[[1,2],[3,4],[5]]
chunk([0, 1, 2, 3, 4, 5], 4) 应该返回 [[0, 1, 2, 3], [4, 5]].
```js
/**
*   Math.ceil 目的是未来算出能分多少组
*   start 每组的起始索引
*   end 每一组的末尾索引值
*   通过循环不停地改变启起始末尾索引，
*   动态的截取 arr.slice 返回一个截取后的数组
*/
const chunk = function (arr,size) {
    let result = [],
        groups = Math.ceil(arr.length/size);
    for(let i=0;i<groups;i++){
        let start = i * size,
            end = start + size;
        result.push(arr.slice(start,end))
    }
    return result
}
console.log(chunk([1,24,3,4,5],3)) //[Array(3), Array(2)]
```
## 数组截断算法挑战
返回一个数组被截断n个元素后还剩余的元素，截断从索引0开始。
```js
const slasher = function(arr,howMany){
    if(howMany > arr.length){
        return []
    }
    // 换个思路,利用slice的负数从后面截取
    return arr.slice(howMany-arr.length)
}
console.log(slasher([1, 2, 3], 0))
// [1, 2, 3]
```
## 数组查询算法挑战
蛤蟆可以吃队友，也可以吃对手。
如果数组第一个字符串元素包含了第二个字符串元素的所有字符，函数返回true。
举例，["hello", "Hello"]应该返回true，因为在忽略大小写的情况下，第二个字符串的所有字符都可以在第一个字符串找到。
["hello", "hey"]应该返回false，因为字符串"hello"并不包含字符"y"。
["Alien", "line"]应该返回true，因为"line"中所有字符都可以在"Alien"找到。
```js
const mutation = function(arr){
    let flag = 0,//变量 后面存储匹配ok的数量
        left = arr[0].toLowerCase(),// 数组第一个元素
        right = arr[1].toLowerCase();//数组第二个元素
    for(var i=0;i<right.length;i++){
        if(left.indexOf(right[i]) > -1){
            // 匹配即加加
            flag++
        }
    }
    // 看是否完全匹配
    if(flag === right.length){
        return true
    }
    return false
}
console.log(mutation(["hello", "llop"]))
```
## 删除数组中特定值算法挑战
真假美猴王！
删除数组中的所有假值。
在JavaScript中，假值有false、null、0、""、undefined 和 NaN
```js
const bouncer = function(arr){
    return arr.filter((item)=>{
        return !!item // !!返回布尔值 用过滤器过滤出为true值
    })
}
console.log(bouncer([1, null, NaN, 2, undefined]))
```
## 去除数组中任意多个值算法挑战
实现一个摧毁(destroyer)函数，第一个参数是待摧毁的数组，其余的参数是待摧毁的值。
```js
const destroyer = function(data){
    let arr = arguments[0],
        oData = [].slice.call(arguments,1);

    for(let i=0;i<oData.length;i++){
        for(let j=0;j<arr.length;j++){
            if(arr[j] === oData[i]){
                arr.splice(j,1);
                j--;
            }
        }
    }
    return arr;
}
console.log(destroyer([3, 5, 1, 2, 2], 2, 3, 5))// [1]
```
## 数组排序并插入值算法挑战
先给数组排序，然后找到指定的值在数组的位置，最后返回位置对应的索引。
举例：where([1,2,3,4], 1.5) 应该返回 1。因为1.5插入到数组[1,2,3,4]后变成[1,1.5,2,3,4]，而1.5对应的索引值就是1。
同理，where([20,3,5], 19) 应该返回 2。因为数组会先排序为 [3,5,20]，19插入到数组[3,5,20]后变成[3,5,19,20]，而19对应的索引值就是2。
```js
const where = function (arr,num) {
    // 先合并
    let res = arr.concat(num);
    // 排序 导出索引
    return res.sort(function(a,b){
        return a - b
    }).indexOf(num)
}
console.log(where([1,2,3,4],1.5))// 1
```
## 位移密码算法挑战(待补)
让上帝的归上帝，凯撒的归凯撒。
下面我们来介绍风靡全球的凯撒密码Caesar cipher，又叫移位密码。
移位密码也就是密码中的字母会按照指定的数量来做移位。
一个常见的案例就是ROT13密码，字母会移位13个位置。由'A' ↔ 'N', 'B' ↔'O'，以此类推。
写一个ROT13函数，实现输入加密字符串，输出解密字符串。
所有的字母都是大写，不要转化任何非字母形式的字符(例如：空格，标点符号)，遇到这些特殊字符，跳过它们。
ex:
rot13("SERR PBQR PNZC") 应该解码为 "FREE CODE CAMP"
rot13("SERR CVMMN!") 应该解码为 "FREE PIZZA!"
```js
占位
```
## 结语
大家如何有更好的解决方法或者疑惑，欢迎补充和交流，可以直接联系我biyuqiwan@163.com
