---
title: State 與 改變 State
date: 2021-07-20 22:18:38
description: State 與 改變 State
categories: Frontend
tags:
  - React
---
## State and Change State
* 一定要透過 `setState` 去修改 state 的狀態

``` js 
import React, {Component} from 'react'

class App extends Component {
  // super() 會去呼叫 Component 的 constructor
  constructor() {
    super()
    this.state = {
      counter: 1
    }
    // 若不做這個動作會產生錯誤 setState is undefined 跟 this有關
    this.handleClick = this.handleClick.bind(this)
  }
  
  handleClick() {
    this.setState({
      counter: this.state.counter ++
    })
  }
  render () {
    return (
      <h1 onClick={this.handleClick}>React</h1>
      <h1>{this.state.counter}
    )
  }
}
```
因為this 的值取決於 function 被呼叫的時候
將 this 對應到 class 實例的方法
``` js 
// 1 在 constructor 終將方法綁定到 this
constructor() {
  ...
  this.handleClick = this.handleClick.bind(this)
}
handleClick() {
  this.setState({
    counter: this.state.counter ++
  })
}
// 2 在 onClick 綁定事件時 bind this
<h1 onClick={this.handleClick.bind(this)}>React</h1>

// 3 透過arrow funtcion
handleClick = () => {
  this.setState({
    counter: this.state.counter ++
  })
}
```