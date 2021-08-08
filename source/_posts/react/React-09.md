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

### shouldComponentUpdate
* 被呼叫的時間點在改變 state 後，才去執行 render function
* 若在此生命週期 `return false` ， state 會被改變但不會觸發 render function 
* 此生命週期會傳入 nextProps 及 nextState 提供作為是否更新 Component 的判斷，適合用在很多 Component 同時存在時判斷哪個 Component 要重新渲染哪個不用

### componentDidMount 
* `componentDidMount` 代表 Cmoponent 已經被 render 到 DOM 。 到這一階段才可使用 `document.getElementBy...` 來取得 DOM 元素。
* 若在還沒觸發 setTimeout 這個 function 前把 Title 這個 Component 隱藏起來，並試著改變 Title 這個 Component 的內容則會發生錯誤，因此需要搭配 componentWillUnmount 來使用。

### componentWillUnmount
* 通常用來把 componentDidMount 做的初始設定在移除 Component 時做一些修改。

### componentDidUpdate
* `setState` 是非同步的若在某一個 funciont 內執行完 `setState` 要直接拿新的 state 須在 `setState` 傳入 callback function 做第二個參數
* 可透過 componentDidUpdate 執行上方的動作，此生命週期會在 state 更新後觸發
* 需注意若 shouldComponentUpdate return false 則 componentDidUpdate 不會被觸發，雖然 state 有變但不會觸發 componentDidUpdate
* componentDidUpdate(prevProps, prevState)

``` js 
class Title extends Component {
  constructor() {
    super()
    this.state = {
      title : 'Hello World'
    }
  }
  componentDidMount() {
    this.timer = setTimeout(() => {
      this.setState({
        title: 'Change Title'
      }, () =>{
        console.log(this.state.title)
      })
    }, 3000)
  }
  componentWillUnmount() {
    clearTimeout(this.timer)
  }
  render() {
    return(
      <div>
        <h1>{this.state.title}</h1>
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
  shouldComponentUpdate(nextProps, nextState) {
    if(this.state.isHide !== nextState.isHide) {
      return true
    }
    return false
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

