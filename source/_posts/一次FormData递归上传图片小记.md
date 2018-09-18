---
title: 一次FormData递归上传图片小记
date: 2017-08-14 20:09:48
tags: [Javascript,'FormData']
author: "LoadingMore"
---
## 问题
最近项目开发中遇到一个多图上传的需求，后台给的接口支持FormData.[这里查看详情FormData](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/FormData)

## 解决
刚开始没有太好的办法，之前用formdata传图，都是固定的个数，对应唯一的filename参数名字，不会发生冲突，所以刚开始想到了for循环...结果就是能上传，但是顺序全部搞乱了，还有就是图片传的重复，缺失严重

## 递归优化
想着for循环也不能控制上传进度，所以采用了递归的思路
```js
// 伪代码
$('#sleect_input').off().on('change',function(e){
    var files = e.target.files,
        resFiles = [];
    // 收集files
    for(var i=0;i<files.length;i++){
        resFiles.push(files[i])
    }

    // 上传
    $('#upLoad').on('click',function(){
        // 递归
        (function uploadFiles(){
            var f = resFiles.shift();
            if(f){
                // 这里创建是为了避免重名导致上传混乱，每次都重新创建新的对象
                var formdata = new FormData();
                formdata.append('file',f);
                $.ajax({
                    url:'XXXXXXXXXXXXXXXXXXXXXX',
                    type: "POST",
                    data: formdata,
                    processData: false,
                    contentType: false,
                }).always(function(){
                    console.log("pending+正在上传");
                    // 继续下一步上传
                    uploadFiles();
                });
            }else{
                console.log("finished+上传完毕要做的事");
            }
        })()
    })
})
```
至此，解决了多图片上传的问题
