---
title: react_props_type
date: 2021-10-15 09:57:38
categories: Frontend
tags:
  - React
---

React.js 提供了一種機制，讓你可以給组件的配置參數加上類型驗證，如下：可以配置 `Comment` 只能接受對象類型的 `comment` 參數，傳數字進來组件就強制報錯。

如下：
* 一開始引入了 `PropTypes`，並且给 `Comment` 组件類添加了類属性 `propTypes`，裡面的内容的意思就是你傳入的 `comment` 類型必須為 object（物件）。
* 可以透過 `isRequired` 關鍵字強制组件某個參數必需傳入
``` js
import React, { Component, PropTypes } from 'react'

class Comment extends Component {
  static propTypes = {
    comment: PropTypes.object.isRequired
  }

  render () {
    const { comment } = this.props
    return (
      <div className='comment'>
        <div className='comment-user'>
          <span>{comment.username} </span>：
        </div>
        <p>{comment.content}</p>
      </div>
    )
  }
}
```

React.js 的 `PropTypes` 提供了一系列的資料類型可以用來配置组件的参数：

``` js
PropTypes.array
PropTypes.bool
PropTypes.func
PropTypes.number
PropTypes.object
PropTypes.string
PropTypes.node
PropTypes.element
...
```

透過 `PropTypes` 給組件的參數做類型限制，可以在幫助我们迅速定位錯誤，這在建置大型應用程式時特别有用；另外，给組件加上 `propTypes`，也讓组件的開發、使用有更明確的規範。

