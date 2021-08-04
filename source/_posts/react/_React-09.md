---
title: React 的 生命週期
date: 2021-08-03 22:20:21
description: React 的 生命週期
categories: Frontend
tags:
  - React
---

### Constructor 的作用

* 每當 Component 生成時，都會被呼叫一次，透過下方例子，透過按鈕切換狀態，可在 `console.log` 看到 `Title` 的 constructor 在 isHide 變 false 時都會觸發。
* 若是透過 CSS display 設定是否顯示 Title 則不會觸發，因為 Title 這個 Componet 在 DOM 並沒有真的被移除。

``` js 
class Title extends Component {
  constructor(props) {
    super(props)
    console.log('Title Created')
  }
  render() {
    return(
      <div>
        <h1>Title</h1>
      </div>
    )
  }
}

class App extends Component{
  constructor() {
    super()
    this.state = {
      isHide: false
    }
    console.log('App Created')
  }
  render() {
    const {isHide} = this.state
    return (
      <div>
        { !isHide ? <Title /> : null }
        <button onClick={ () =>{
          this.setState({
            isHide: !this.state.isHide
          })
        }          
        }>Toggle</button>
      </div>
    )
  }
}

export default App
```