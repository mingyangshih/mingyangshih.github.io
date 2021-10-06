---
title: React controlled 與 un-controlled 的差異
date: 2021-08-24 21:37:31
description: React controlled 與 un-controlled 的差異
categories: Frontend
tags:
  - React
---

### React controlled 與 un-controlled 的差異

React 對於表單會有兩種不同的 component 一種是 un-controlled component
* uncontrolled component : component 沒有跟 state 有相對應的 component 稱之。可透過 ref 取得 DOM 元素的值。


``` js
import React, {Component} from 'react'

class App extends React {
  constructor(props){
    super(props)
    // 就會是對應到的 DOM 物件 (uncontrolled component)
    this.input = React.createRef()
    // controlled component 透過 state 去對應 UI
    this.state = {
      name: ''
    }
  }
  clickhandler = () => {
    alert(this.input.current.value)
  }
  changehandler = (e) => {
    this.setState({
      // 一定要這樣刮起來，不然會被解譯成要改變 e.target.name
      [e.target.name] = e.target.value
    })
  }
  
  render() {
    return(
      <div>
        <div>
          name: <input name="name" type="text" onChange = {this.changehandler} value={this.state.name}>
          name: <input type="text" ref = {this.input}>
        </div>
        <div>
          <input type="submit" onClick={this.clickhandler} >
        </div>
      </div>    
    )
  }
}
```