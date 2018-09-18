---
title: 来看看select是个什么东东
date: 2017-01-23 11:30:12
tags: [javascript,select]
categories: Javascript
---
> ### 关键词：select option options selectedIndex remove add

<!-- more -->
### 属性
* options：返回包含下拉列表中的所有选项的一个数组
* name：设置或返回下拉列表的名称。
* selectedIndex：设置或返回下拉列表中被选项目的索引号。
* add()：向下拉列表添加一个选项。
* remove()：从下拉列表中删除一个选项。
* onchange：当改变选择时调用的事件句柄。

### 实战
#### HTML结构
```HTML
<select class="s1">

</select>
```
####  selectedIndex, options, onchange
写的时候注意不要写的错单词：
```javascript
var s1 = document.querySelector('.s1');
s1.onchange = function(){
    //当前选中option 所在索引
    //也可写成this.options.selectedIndex
    console.log(this.selectedIndex);
}
```
#### add()
动态的添加option
```javascript
/*
* elem selector的id 或者 clas
* obj 要添加的option选项 提供批量添加
    格式：[
            {con:"option文本值",item:"option value值"},
            {con:"option文本值",item:"option value值"}
        ]
*/
function addOption(elem,obj){
    var el = document.querySelector(elem);
    for(i in obj){
        var op = new Option(obj[i].con,obj[i].item);
        el.add(op);
    }
}
//测试
var obj = [
    {con:"微信",item:"-1"},
    {con:"支付宝",item:"-2"},
    {con:"银联",item:"-3"},
    {con:"拒绝",item:"-5"},
    {con:"接受",item:"-6"}
];
//调用
//可以在浏览器跑跑看效果了
addOption('.s1',obj);
```
![enter description here][1]

#### remove()
动态的删除指定option
```javascript
/*
* elem selector的id 或者 class
* obj  要添加的option选项 提供批量删除
*      可根据需要 更改函数 指定删除匹配的option文本
*      本文默认匹配option  value值
*      格式一 字符串 value值  "-3" 目前只支持一个
*      推荐：格式二：[
*             {val:"option value值"},
*            {val:"option value值"},
*            {val:"option value值"}
*         ]
*/
function removeOption(elem,opText) {
    var el = document.querySelector(elem);
    for(var i=0;i<el.options.length;i++){
        if(typeof opText === 'string'){
            //此处可根据需要改成匹配 text
            //el.options[i].text 即option文本值
            //string 仅支持一次删除一个
            if(el.options[i].value === opText){
                el.options.remove(i);
            }
        }
        if(typeof opText === 'object'){
            for(j in opText){
                //此处可根据需要改成匹配 text
                //el.options[i].text 即option文本值
                if(el.options[i].value === opText[j].val){
                    el.options.remove(i);
                }
            }
        }
    }
}
//测试 上文中添加的value 值
//推荐使用对象，可批量删除
var text = [
    {val:"-1"},
    {val:"-2"}
];
//调用
//可以在浏览器跑跑看效果了
removeOption('.s1',text);
```
![enter description here][2]
#### item(index)
用来选择option
```javascript
var s1 = document.querySelector('.s1');
s1.item(0);//选中第一个option
```
### Reference
* 1.[小胡子哥][3]
* 2.[MDN][4]

  [1]: http://oiukswkar.bkt.clouddn.com/select1.png
  [2]: http://oiukswkar.bkt.clouddn.com/select2.png
  [3]: http://www.barretlee.com/blog/2013/04/11/cb-options_add_and_remove/
  [4]: https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLSelectElement
  [5]: http://oiukswkar.bkt.clouddn.com/select.png
