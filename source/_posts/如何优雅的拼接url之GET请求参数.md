---
title: 如何优雅的拼接url之GET请求参数
date: 2017-01-17 20:08:07
tags: [javascript, url, 'URL拼接', '解析URL']
categories: Javascript
---
> 请求的URL后面带参数在项目中是很常见的，常用在的地方比如跳转到新页面或者搜索关键词等，刚好项目用到，就写了我认为比较简便的方法提取参数

<!-- more -->

### 最常见的形式

```javascript
 url?arg1=value1&arg2=value2&arg3=value3...
```
### 如何拼接URL？
假如一个查询系统有很多字段,刚好需要拼接URl的形式进行查询，此刻我们可以采用对象过滤的方法进行转换：
```js
const searchParam = {
    name: 'Bob',
    age: 20,
    address: '',
    phone: 18888888888
}

const transformParamToUrl = (param) => {
    const tempObj = []
    if (!param || typeof param !== 'object') {
        return
    }

    for (let i in param) {
        if (param.hasOwnProperty(i) && param[i]) {
            tempObj.push(i)
            tempObj.push('=')
            tempObj.push(param[i])
            tempObj.push('&')
        }
    }
    // remove the last field &
    tempObj.pop()
    return tempObj.join('')
}
transformParamToUrl(searchParam)
// name=Bob&age=20&phone=18888888888
```

我们得到了拼接好的URL，并且未将无效值加入.

### 解析URL参数

我们拼接了URl，下面讲下如何解析URL参数.
```js
const flatten = (arr) => 
    arr.reduce((initial, curr, index) => 
    Array.isArray(curr) ? initial.concat(flatten(curr)) : initial.concat(curr), [])

const parseQuery = (query) => {
    const regexp = /(\w+)=([^&]+)/g
    const result = {}
    let match

    while(match = regexp.exec(query)) {
        let key = match[1], value = match[2].replace(/\n/g, '')
        result[key] ? result[key] = flatten([result[key], value]) : result[key] = value
    }

    return result
}

// example:
const query = `
https://www.baidu.com/s?ie=utf8&f=8&f=90&rsv_bp=1&rsv_idx=1&rsv_bp=1&tn=baidu&wd=%E6%B0%B4%E7%94%B5%E8%B4%B9&rsv_pq=c7797024000434a8&rsv_t=b786FJnGwOOxPk7E7gsn1VbYHpmSP93UpP1470GL9ajYJkd09MOyBzSTsVk&rqlang=cn&rsv_enter=1&rsv_dl=tb&rsv_sug3=5&rsv_sug1=4&rsv_sug7=101&rsv_bp=234&rsv_sug2=0&inputT=1760&rsv_sug4=2010&jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmcm9tX3VzZXIiOiJCIiwidGFyZ2V0X3VzZXIiOiJBIn0.rSWamyAYwuHCo7IFAgd1oRpSP7nzL7BF5t7ItqpKViM
`.split('?')[1]

parseQuery(query)

// { 
//   ie: 'utf8',
//   f: [ '8', '90' ],
//   rsv_bp: [ '1', '1', '234' ],
//   rsv_idx: '1',
//   tn: 'baidu',
//   rsv_pq: 'c7797024000434a8',
//   rsv_t: 'b786FJnGwOOxPk7E7gsn1VbYHpmSP93UpP1470GL9ajYJkd09MOyBzSTsVk',
//   rqlang: 'cn',
//   rsv_enter: '1',
//   rsv_dl: 'tb',
//   rsv_sug3: '5',
//   rsv_sug1: '4',
//   rsv_sug7: '101',
//   rsv_sug2: '0',
//   inputT: '1760',
//   rsv_sug4: '2010' 
// }
```
我们得到了预期的解析数据，并且相同的key进行了数组化存储.

### 在线小Demo
Demo仅做参考，请看代码请看上面👆.
[DEMO](https://biyuqi.github.io/demo/src/html/url.html)