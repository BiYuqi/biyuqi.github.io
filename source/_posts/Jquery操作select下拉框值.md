---
title: Jquery操作select下拉框值
date: 2017-08-16 00:36:00
tags: [Jquery,'select']
author: "LoadingMore"
---
## 前言
> 最近项目中操作select的option值得场景还是很多的，在此记录一下,以免忘记

## 结构
```html
<select id="selected">
    <option value="1" data-id="1000"></option>
    <option value="2" data-id="1001"></option>
    <option value="3" data-id="1002"></option>
    <option value="4" data-id="1003"></option>
</select>
```
## 操作
```js
// change事件中进行操作
$('#selected').on('change',function(){
    // 获取被选中的option 自定义属性
    var selectedOpt = $('#selected').find('option:selected').attr('data-id')

    //获取下拉框选中项的value属性值
    var selectedVal = $('#selected').val()

    //获取下拉框选中项的index属性值
    var selectedIndex = $("#selected").get(0).selectedIndex

    console.log(selectedIndex)
})

//设置下拉框值为4的option选中
function setOpt(){
    $('#selected option[value=4]').attr('selected','selected')
}
// setOpt()

//在下拉框最后添加一个选项
function addOpt(){
    $('#selected').append('<option value="5" data-id="1004">蘑菇云</option>')
}
// addOpt()

//移除下拉框最后一个选项
function removeLastOpt(){
    $('#selected option:last').remove()
}
// removeLastOpt()

// 获取最后一个下拉框自定义属性值
var selectMaxIndex = $("#selected option:last").attr("data-id");
```
