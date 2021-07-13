---
title: React小書 Note 03
date: 2021-07-12 20:36:04
description: React小書 Learning Note 前端組件化（三）：抽像出公共組件類
categories: Frontend
tags: 
  - React
---

<font color="yellow" >React 核心觀念，改變state 時會trigger re-render </font>
### 拆出抽象模式
為了讓程式更靈活，可以套用在不同component，把這種模式抽像出來，產生一個 Component Class：

``` js
class Component {
  setState (state) {
    const oldEl = this.el
    this.state = state
    this.el = this._renderDOM()
    if (this.onStateChange) this.onStateChange(oldEl, this.el)
  }

  _renderDOM () {
    this.el = createDOMFromString(this.render())
    // 物件實體有onClick這個方法時，註冊click事件到這個element上
    if (this.onClick) {
      this.el.addEventListener('click', this.onClick.bind(this), false)
    }
    return this.el
  }
}
```
這個是一個Father Class Component，所有的組件都可以繼承這個父類來建造。它定義的兩個方法，一個是我們已經很熟悉的 `setState` ；一個是私有方法 `_renderDOM` 。`_renderDOM` 方法會調用 `this.render` 來構建DOM元素並且監聽 `onClick` 事件。所以，組件子類繼承的時候只需要實作返回HTML字符串的`render` 方法及 `onClick` 方法即可 。

還有一個額外的 `mount` 的方法，其實就是把組件的DOM元素插入頁面，並且在 `setState` 的時候更新頁面：
``` js
const mount = (component, wrapper) => {
  wrapper.appendChild(component._renderDOM())
  component.onStateChange = (oldEl, newEl) => {
    wrapper.insertBefore(newEl, oldEl)
    wrapper.removeChild(oldEl)
  }
} 
// 繼承Father Class Component 的 component
class LikeButton extends Component {
  constructor () {
    this.state = { isLiked: false }
  }

  onClick () {
    this.setState({
      isLiked: !this.state.isLiked
    })
  }

  render () {
    return `
      <button class='like-btn'>
        <span class='like-text'>${this.state.isLiked ? '取消' : '点赞'}</span>
        <span>👍</span>
      </button>
    `
  }
}

mount(new LikeButton(), wrapper)
```
### 傳入自定義的資料
在實際開發時，時常需要給Component傳入一些自定義的資料，如：樣式、函式...等。
可以給Component Class和它的 Children Class 都傳入一個參數props，作為Component的配置參數。修改Component的constructor為：

Father Component
``` js
class Component {
  constructor (props = {}) {
    this.props = props
  }
  setState (state) {
    const oldEl = this.el
    this.state = state
    this.el = this._renderDOM()
    if (this.onStateChange) this.onStateChange(oldEl, this.el)
  }

  _renderDOM () {
    this.el = createDOMFromString(this.render())
    // 物件實體有onClick這個方法時，註冊click事件到這個element上
    if (this.onClick) {
      this.el.addEventListener('click', this.onClick.bind(this), false)
    }
    return this.el
  }
}
```
繼承的時候通過 `super(props)` 把 `props` 傳給父類，這樣就可以通過 `this.props` 獲取到配置參數：
``` js
class LikeButton extends Component {
  constructor (props) {
    super(props) // 加這一行
    this.state = { isLiked: false }
  }

  onClick () {
    this.setState({
      isLiked: !this.state.isLiked
    })
  }
  //  按鈕樣式改成根據傳入的 props 資料作修改
  render () {
    return `
      <button class='like-btn' style="background-color: ${this.props.bgColor}">
        <span class='like-text'>
          ${this.state.isLiked ? '取消' : '点赞'}
        </span>
        <span>👍</span>
      </button>
    `
  }
}
mount(new LikeButton({ bgColor: 'red' }), wrapper)
```

### 實作前端組建話總結
* 組件化可以幫助我們解決前端結構的複用性問題，整個頁面可以由這樣的不同的組件組合、嵌套構成。
* 每個組件都有自己的狀態（上面的HTML 結構和內容）行為，組件的狀態和行為可以由數據狀態（state）和配置參數（props）決定，狀態和配置參數的改變都會影響組件的顯示形態。