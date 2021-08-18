---
title: Component, Render function, JSX 語法, React 事件機制
date: 2021-07-19 22:00:44
description: Component, Render function, JSX 語法, React 事件機制
categories: Frontend
tags:
  - React
---
## Component
* 在 react 內每個東西都是一個 Component
* 在每個 Component 內一定要有 render function 不然會有 error
* 每個 render funtion 必須要回傳 JSX 元素。要注意的是，必須要用一個外層的 JSX元素 把所有內容包裹起來。回傳並列多個JSX 元素是不合法的。
* Component 命名第一個字母一定要大寫

``` js 
// 寫 React 一定要引用這兩個
import React, {Component} from 'react'
import ReactDOM from 'react-dom'
class App extends Component {
  render() {
    const variable = 'Component Variable'
    const header = 'header'
    const hello = <h1>Hello React Test !!!</h1>
    const style= {
      backgroundColor : 'red'
    }
    return ( //如果不加( 會變return undefined
      <div className={`${header} footer`} style={style}>
        <Title />
        {hello}
        <h3>{variable}</h3>
        <h4> {(() => {return 1+2})()}</h4>
      </div>
    )
  }
}
ReactDOM.render((
  <App/>
),document.getElementById('app'))

export default App
```

## JSX 語法
* Render Component ` <Title /> `
* class 要改成 className `<div className="title">...</div>`，因為 class 是 JS 的關鍵字；`for` 屬性也是，需改成 `<label htmlFor='male'>Male</label>`。
* 要加 CSS 的話要傳入一個 Object（物件）`<div className={header} style={style}>`， header 不是物件所以被當成JS解析，style 傳入 style的物件。
* CSS 屬性若有 dash line 要改成 camelCase `fontSize` 。
* JSX 是 JavaScript 語言的一語法擴充，長得像 HTML，但並不是 HTML。
* React.js 可以用 JSX 撰寫 Component。
* JSX 在編譯的时候會變成成相對應的 JavaScript 物件。
* react-dom 負責把這個用来描述 UI 信息的 JavaScript 物件變成 DOM 元素，且 render 到頁面上。

``` js 
class Header extends Component {
  render () {
    return (
      <div>
        <h1 className='title'>React</h1>
      </div>
    )
  }
}
ReactDOM.render(
  <Header />,
  document.getElementById('root')
)

//  編譯後
class Header extends Component {
  render () {
    return (
     React.createElement(
        "div",
        null,
        React.createElement(
          "h1",
          { className: 'title' },
          "React"
        )
      )
    )
  }
}

ReactDOM.render(
  React.createElement(Header, null), 
  document.getElementById('root')
);
```

## React 事件機制
``` js 
...
function test() {
  alert('test')
}
return ( //如果不加( 會變return undefined
  <div className={`${header} footer`} style={style}>
    <Title />
    {hello}
    <h3 onClick={test}>{variable}</h3>
    <h4> {(() => {return 1+2})()}</h4>
  </div>
)
...
```

## event 對象
和一般瀏覽器一樣，事件監聽函數會被自動傳入一個 event 物件，這個物件和普通的瀏覽器 event 物件所包含的方法和屬性都基本一致。不同的是 React.js 中的event 物件並不是瀏覽器提供的，而是它自己內部所構建的。React.js 將瀏覽器原生的 event 物件封裝，對外提供統一的API和屬性，這樣就不用考慮不同瀏覽器的兼容性問題。
```js 
class Title extends Component {
  handleClickOnTitle (e) {
    console.log(e.target.innerHTML)
  }

  render () {
    return (
      <h1 onClick={this.handleClickOnTitle}>React</h1>
    )
  }
}
```
* React Component 新增事件監聽是很簡單的事情，你只需要使用 React.js 提供了一系列的 on* 方法即可。

* React.js 會給每個事件監聽傳入一個 event 物件，這個物件提供的功能和瀏覽器提供的功能一致，而且它是兼容所有瀏覽器的。

* React.js 的事件監聽方法需要手動 bind 到當前實例，這種模式在React.js中非常常用。

## React 渲染機制與 Virtual DOM

* 在 React 內透過 Virtual DOM 處理全部重新渲染的問題，若有狀態被修改，React 只會重新渲染被修改的部分。

* Call render function 後會先產生 Virtual DOM 之後才會真的產生 DOM