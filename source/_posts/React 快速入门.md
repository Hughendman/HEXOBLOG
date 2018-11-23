---
title: React 快速入门
categories:
  - react
  - wehotel
tags:
  - react
  - wehotel
keywords: react
abbrlink: 61aa4cfb
date: 2018-11-13 16:15:00
---

## React JSX
###  JSX介绍
它是一种JavaScript语法扩展，在React中可以方便地用来描述UI。
### 使用 JSX

```
ReactDOM.render(
    <h1>Hello, world!</h1>
);

```

### JavaScript 表达式

表达式写在花括号 {} 中:

```
ReactDOM.render(
    <div>
      <h1>{1+1}</h1>
    </div>
);   
```

```
return (
    <div>
        <h1>{i == 1 ? 'True!' : 'False'}</h1>
    </div>
)

```

### 样式

```
var myStyle = {
    fontSize: 100,
    color: '#FF0000'
};
ReactDOM.render(
    <h1 style = {myStyle}>Hello World!</h1>
);

```
index.less
```
.db {
  background-color: #49B79F;
}
```
```
import style from './index.less';
<div>
    <h1>{i == 1 ? 'True!' : 'False'}</h1>
    <h1 className={style.db}>Hello World!</h1>
</div>

```

### 注释

注释需要写在花括号中

```
{/*注释...*/}

```

### 数组

```
let arr = [
    <h1>yinxs</h1>,
    <h2>你好</h2>,
];
return (
    <div>
        {arr}
    </div>
)

```

### if/for/function

```

render() {
    function getGreeting(user) {
        if (user) {
            return <h1>Hello, {user}!</h1>;
        }
        return <h1>Hello, Stranger.</h1>;
    }
    return getGreeting("yinxs")
}

```
### 属性值

```
const element = <img src={user.avatarUrl}></img>;

```

## React 组件
### 组件
```
import {Component} from "react";
import {createPortal} from "react-dom";

export default class Hello extends Component{
    render(){
        return createPortal(
            <div>
                <h1>Hello,world!</h1>
            </div>
            ,
            document.getElementsByTagName('body')[0]
        );
    }
}

```

### 使用

```
import React, { Component } from 'react';
import Hello from '../../public/components/hello/';

export default class Text extends Component{
  render() {
    return (
        <Hello></Hello>
    )
  }
}

```
## React State(状态)

React 把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。

React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。

以下实例创建一个名称扩展为 React.Component 的 ES6 类，在 render() 方法中使用 this.state 来修改当前的时间。

添加一个类构造函数来初始化状态 this.state，类组件应始终使用 props 调用基础构造函数。


## React Props

```
import {Component} from "react";
import {createPortal} from "react-dom";
import PropTypes from "prop-types";


export default class Btn extends Component{
    static propTypes = {
        name: PropTypes.bool.isRequired,
    };
    constructor(props){
        super(props);
    }
    render(){
        let myStyle = {
            width: 50,
            height: 20,
            backgroundColor: '#fff'
        };
        return createPortal(
            <div>
                <button style={myStyle}>{this.props.name}</button>
            </div>
            ,
            document.getElementsByTagName('body')[0]
        );
    }
}

```

```
import React, { Component } from 'react';
import Btn from '../../public/components/button/';

export default class Contend extends Component{
    handle(){

    }
    render() {
        function getGreeting(user) {
            if (user) {
                return <h1>Hello, {user}!</h1>;
            }
            return <h1>Hello, Stranger.</h1>;
        }
        return (
            getGreeting("yinxs"),
            <Btn name={"点击"}>

            </Btn>
        )
    }
}


```

## 事件处理

```
import {Component} from "react";
import {createPortal} from "react-dom";
import PropTypes from "prop-types";


export default class Btn extends Component{
    static propTypes = {
        name: PropTypes.bool.isRequired,
    };
    constructor(props){
        super(props);
    }
    render(){
        let myStyle = {
            width: 50,
            height: 20,
            backgroundColor: '#fff'
        };
        function click() {
            alert("按钮被点击了")
        }
        return createPortal(
            <div>
                <button style={myStyle} onClick={click}>{this.props.name}</button>
            </div>
            ,
            document.getElementsByTagName('body')[0]
        );
    }
}

```