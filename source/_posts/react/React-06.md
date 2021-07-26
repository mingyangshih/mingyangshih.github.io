---
title: React State 與 改變 State
date: 2021-07-20 22:18:38
description: State 與 改變 State
categories: Frontend
tags:
  - React
---
## State and Change State
* `state` 用來儲存可改變的狀態。
* 當我們要改變組件的狀態的時候，不能直接用 `this.state = xxx` 這種方式來修改，這樣做 React.js 沒辦法知道你修改了組件的狀態，就沒有辦法更新頁面。所以，一定要使用 React.js 提供的 `setState` 方法，它接受一個物件或者函數作為參數。當我們使用 `setState` ，React.js會更新組件的狀態 `state`，並且重新調用 `render` 方法，然後再把 `render` 方法所渲染的最新的內容顯示到頁面上。

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

## setState 接受函數參數
* 在 `React` 中使用 `setState` 時，<font color="yellow">React.js並不會馬上修改 `state`</font>，而是把它放到一個更新的 queue，稍後才會從 queue 中把新的狀態提取出來合併到 `state` 當中，然後再觸發組件更新。

所以如果你想在 `setState` 之後使用新的 `state` 來做計算就會有問題，下方的程式執行一次預期會得到 `4` ，但得到的結果是 `2`，

``` js
handleClickOnLikeButton () {
  constructor(){
    super()
    this.state = {
      counter: 1
    }
    this.handleClick=this.handleClick.bind(this)
  }
  handleClick = () => {
    this.setState({
      counter: this.state.counter + 1
    })
    this.setState({
      counter: this.state.counter + 1 
    })
    this.setState({
      counter: this.state.counter + 1
    })
  }
}
```
若為了讓 `counter` 執行完 `handleClick` 後變成 `4` ，可以使用 `setState` 的第二種使用方式，使用一個函數作為參數，React.js 會把上一個 `setState` 的結果傳入這個函數，就可以使用該結果進行運算、操作，然後回傳一個物件作為更新 `state` 的對象：

``` js 
handleClick = () => {
  this.setState((prevState) => {
    return {
      counter: prevState.counter + 1
    }
  })
  this.setState((prevState) => {
    return {
      counter: prevState.counter + 1
    }
  })
  this.setState((prevState) => {
    return {
      counter: prevState.counter + 1
    }
  })
}
```
雖然上方的程式執行了三次 `setState`，但是實際上 `Component` 只會重新渲染一次，而不是三次；因為在 React.js 中會把 `JavaScript` 事件循環中同一個消息中的 `setState` 都進行合併以後再重新渲染組件。