---
title: React State 與 props
date: 2021-07-26 22:40:28
description: React State, props and defaultProps
categories: Frontend
tags:
  - React
---

### props 用法

* 可透過 Component 的屬性傳入參數，並使用 `this.props` 使用傳入的資料。
* `props` 是別人傳給你的，`state` 是自己的狀態。
* 若使用 `<Title> {this.state.counter} </Title>` 裡面的值(...)可透過 `this.props.children` 秀出來。

``` js
import React, {Component} from 'react'

export class Title extends Component {
  render() {
    return (
      <div>
        <h2>I am Title</h2>
        <h3>{this.props.value}</h3>
        <h4>{this.props.children}</h4>
      </div>
      
    )
  }
}
class App extends Component { // Class Component
  constructor(){
    super()
    this.header = 'header'
    this.state = {
      counter: 1
    }
  }
  handleClick = () => {
    this.setState((prevState) => {
      return {
        counter: prevState.counter + 1
      }
    })
  }
  render() {
    return ( //如果不加( 會變return undefined
      <div >
        <Title value={this.state.counter}/>
        <Title>
          {this.state.counter}
        </Title>
      </div>
    )
  }
}
```

### 也可以透過 `props` 傳入 function
``` js
class App extends Component {
  render () {
    return (
      <div>
        <Title
          onClick={() => console.log('Click on like button!')}/>
      </div>
    )
  }
}
// 可透過下方使用傳入 function
handleClickOnLikeButton () {
  this.setState({
    isLiked: !this.state.isLiked
  })
  if (this.props.onClick) {
    this.props.onClick()
  }
}
```

### 默認配置 defaultProps
`defaultProps` 作為 `App` 類屬性，讓我們就不需要判斷配置屬性是否傳值進來，如果沒有值傳進來，會直接使用 `defaultProps`。
``` js 
class App extends Component {
  static defaultProps = {
    TextCancel: '取消',
    TextConfirm: '確認'
  }

  constructor () {
    super()
    this.state = { isLiked: false }
  }

  handleClickOnLikeButton () {
    this.setState({
      isLiked: !this.state.isLiked
    })
  }

  render () {
    return (
      <button onClick={this.handleClickOnLikeButton.bind(this)}>
        {this.state.isLiked
          ? this.props.TextConfirm
          : this.props.TextCancel} 👍
      </button>
    )
  }
}
```

### props 不能被改變

`props` 一旦傳入 `component` 就不能改變。

``` js
handleClickOnLikeButton () {
  this.props.likedText = '取消'  // 這樣會報 error
  this.setState({
    isLiked: !this.state.isLiked
  })
}

```


### functional component

* `porps` 會是 functional component 的第一個參數

``` js
function Title(props) {
  return (
    <div>
      <h2>I am Title</h2>
      <h3>{props.value}</h3>
      <h4>{props.children}</h4>
    </div>
  )
}
```
