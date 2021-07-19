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
* Component 命名第一個字母一定要大寫

``` js 
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

export default App
```

## JSX 語法
* Render Component ` <Title /> `
* class 要改成 className `<div className="title">...</div>`
* 要加 CSS 的話要傳入一個 Object（物件）`<div className={header} style={style}>`， header 不是物件所以被當成JS解析，style 傳入 style的物件。
* CSS 屬性若有 dash line 要改成 camelCase `fontSize` 。

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