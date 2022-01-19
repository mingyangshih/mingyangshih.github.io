---
title: React Higher Order Component
date: 2021-12-13 08:53:47
categories: Frontend
description: React Higher Order Component
tags:
  - React
---

### 什麼是 Higher Order Component

Higher Order Component 是一個 function，傳入一個Component，會回傳一個新的Component。

``` js
const NewComponent = higherOrderComponent(OldComponent)
```

NewComponent 會根據第二個參數 name 在 `componentWillMount` 從 LocalStorage 載入數據，並且 `setState` 到自己的 `state.data` 中，而渲染的時候將 `state.data` 透過 `props.data` 傳給 WrappedComponent。
``` js 
// src/wrapWithLoadData.js
import React, { Component } from 'react'

export default (WrappedComponent, name) => {
  class NewComponent extends Component {
    constructor () {
      super()
      this.state = { data: null }
    }

    componentWillMount () {
      let data = localStorage.getItem(name)
      this.setState({ data })
    }

    render () {
      return <WrappedComponent data={this.state.data} />
    }
  }
  return NewComponent
}

```

假如 InputWithUserName 的功能需求是載入的時候從 `LocalStorage` 裡面載入 `username` 字段作為 `<input />` 的 `value` 值，有了 wrapWithLoadData，可以很容易地做到這件事情。
``` js
import wrapWithLoadData from './wrapWithLoadData'

class InputWithUserName extends Component {
  render () {
    return <input value={this.props.data} />
  }
}

InputWithUserName = wrapWithLoadData(InputWithUserName, 'username')
export default InputWithUserName
```

只需要定義一個簡單的 InputWithUserName，它會把 `props.data` 作為 `<input />` 的 `value` 值。然後把這個组件和 `username` 傳给 `wrapWithLoadData`，`wrapWithLoadData` 會 `return` 一個新的Component，用這個新的Component取代原来的 InputWithUserName，然後再export module。
别人用這個Component時實際上是用了被加工過的Component：

``` js
import InputWithUserName from './InputWithUserName'

class Index extends Component {
  render () {
    return (
      <div>
        用户名：<InputWithUserName />
      </div>
    )
  }
}
```