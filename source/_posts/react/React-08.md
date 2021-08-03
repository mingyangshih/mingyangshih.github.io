---
title: React Comonent 間的溝通
date: 2021-07-28 22:20:28
description: 如何從 Child Component 改變 Father Component 的資料
categories: Frontend
tags: 
  - React
---

* 只有 Componet 才能改變自己的 State

如何從 Child Component 改變 Father Component 的資料：
1. 將方法透過 props 傳入 Child Component 
2. 在 Child Component 宣告事件並綁定傳入的 props 函式
3. 當觸發事件時，會呼叫傳入了 函式，改變 Father Component State 狀態並重新 render

``` js
class Title extends Component {
  render() {
    return (
      <div>
        <h3>{this.props.value}</h3>
      </div>
      
    )
  }
}
class ComponentContact extends Component {
  render() {
    return (
      <div>
        <button onClick={this.props.handleClick}>Click</button>
      </div>
    )
  }
}
class App extends Component {
  constructor(){
    super()
    this.state = {
      counter: 1,
    }
    this.handleClick=this.handleClick.bind(this)
  }
  handleClick = () => {
    this.setState((prevState) => {
      return {
        counter: prevState.counter + 1
      }
    })
  }
  render() {
    const variable = 'Component Variable'
    const style= {
      backgroundColor : 'red'
    }
   
    return ( //如果不加( 會變return undefined
        <div className={`footer`} style={style}>
          <Title value={this.state.counter}/>
          <ComponentContact handleClick = {this.handleClick}/>
        </div>
    )
  }
}

export default App
```