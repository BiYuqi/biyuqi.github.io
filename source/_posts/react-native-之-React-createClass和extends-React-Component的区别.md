---
title: React.createClass和extends React.Component的区别
date: 2016-12-18 21:54:19
tags: [React,React-native]
categories: React
---
> createClass本质上是一个工厂函数，extens的方式更加接近最新的ES6规范的class写法。两种方式在预防上的差别主要体现在方法的定义和静态属性的声明上.createClass方式的方法定义使用逗号隔开，因为createClass本质上是一个函数，传递给它的是一个Object；而class的方式定义方法时务必不要使用逗号隔开，这是ES6class 的语法规范。


<!-- more -->
## React.createClass和extends Component的区别主要在于：
* 1.语法区别
* 2.模块的导出
* 3.状态的区别
* 4.实例函数／回调函数的绑定(this)
* 5.propType 和 getDefaultProps

## 一、语法区别
React.createClass
```javascript
import React from 'react';
const ListDemo = React.create({
	render(){
		return (
			<div></div>
		)
	}
})；
module.exports = ListDemo;
```
React.Component
```javascript
import React from 'react';
class ListDemo extends React.Component {
	constructor(props) {
		super(props)
	}
	render(){
		return (
			<div></div>
		)
	}
}
export default ListDemo;
```
后一种方法使用ES6的语法，用constructor构造器来构造默认的属性和状态。类中方法的定义不再需要尾随 function，而且方法之间不需要以 , 分隔.

## 二、模块的导出
> 在 ES6 语法中，模块的导出使用 export default 代替 module.exports，如下所示：


```javascript
// ES5 语法
var ListDemo = React.createClass({
    ...
});

module.exports = ListDemo;

// ES6 语法
class ListDemo extends React.Component {
    ...
}
export default ListDemo
```
## 三、状态的区别
React.createClass
使用 ES5 语法时，React Native 组件的状态变量是在 getInitialState 函数中初始化的，如下所示：
```javascript
import React from 'react';
const ListDemo = React.create({
	// return an object
	getInitialState(){
	        return {
	            isEditing: false
	        }
   	 },
	render: function(){
		return (
			<div></div>
		)
	}
})；
module.exports = ListDemo;
```
React.Component
到了 ES6 语法中，React Native 团队修改了状态变量的初始化方式，取消了单独的 getInitialState 函数，将初始化放在构造函数中，并提供 this.state 实例变量用来存储状态变量，如下所示：
```javascript
import React from 'react';
class ListDemo extends React.Component {
	constructor(props) {
		super(props);
		this.state = {
           		 scrollTop: new Animated.Value(0),
        	};
	}
	render(){
		return (
			<div></div>
		)
	}
}
export default ListDemo;
```
## 四、实例函数／回调函数的绑定(this)
React.createClass：会正确绑定this
ES5 语法中，React Native 的 createClass 函数提供的一个便利的功能是自动绑定类作用域中的函数到组件实例，例如 createClass 里面定义的函数可以通过 this 直接指向本组件的某个成员函数
```javascript
import React from 'react';
let ListDemo = React.createClass({
    _handleClick : function() { // 这个方法将作为回调函数使用
        console.log('onClick');
    },
    render : function() {
        return (
            <View style={styles.container}>
                <Text style={styles.normal}
                    onPress={this._handleClick}>
                    确定
                </Text>
            </View>
        );
    },
});
module.exports = ListDemo;
```
React.Component
到了 ES6 的 class 定义中，开发者必须手动进行回调函数的绑定操作，React Native 团队推荐在组件的构造函数中进行，如下所示：
```javascript
import React from 'react';
class ListDemo extends React.Component {
    constructor(props) {
        super(props);
    }
    _handleClick() {
        console.log('onClick');
    }
    render() {
        return (
            <View style={styles.container}>
                <Text style={styles.normal}
                    onPress={this._handleClick.bind(this)}>
                    确定
                </Text>
            </View>
        );
    }
}
export default ListDemo;
```
> 我们还可以在 constructor 中来改变 this.handleClick 执行的上下文，这应该是相对上面一种来说更好的办法，万一我们需要改变语法结构，这种方式完全不需要去改动 JSX 的部分：

```javascript
import React from 'react';
class ListDemo extends React.Component {
    constructor(props) {
        super(props);
        // 手动执行绑定操作
        this._handleClick = this._handleClick.bind(this);
    }
    _handleClick() {
        console.log('onClick');
    }
    render() {
        return (
            <View style={styles.container}>
                <Text style={styles.normal}
                    onPress={this._handleClick}>
                    确定
                </Text>
            </View>
        );
    }
}
export default ListDemo;
```
## 五、propType 和 getDefaultProps
React.createClass：通过proTypes对象和getDefaultProps()方法来设置和获取props.
```javascript
import React from 'react';

const ListDemo = React.createClass({  
  propTypes: {
    name: React.PropTypes.string
  },
  getDefaultProps() {
    return {

    };
  },
  render() {
    return (
      <div></div>
    );
  }
});

export default ListDemo;
```
React.Component：通过设置两个属性propTypes和defaultProps
```javascript
import React form 'react';
class ListDemo extends React.Component{
    static propTypes = { // as static property
        name: React.PropTypes.string
    };
    static defaultProps = { // as static property
        name: ''
    };
    constructor(props){
        super(props)
    }
    render(){
        return <div></div>
    }
}
export default ListDemo;
```

** 最近使用RN比较多,慢慢填坑路... 慢慢写吧!**
