---
title: React小書 Note 02
date: 2021-07-04
description: React小書 Learning Note (前端組件化二) 優化DOM操作
categories: Frontend
tags: 
  - React
---
<font color="yellow" >React 核心觀念，改變state 時會trigger re-render </font>

在上一篇的changeLikeText函數中包括了DOM的操作；數據狀態改變會導致我們去更新頁面的內容，所以當component依賴的數據狀態變多時，component內基本上都是DOM的操作。

代碼中混雜著DOM的操作不是太好的實踐，因為手動管理數據和DOM 之間的關係會導致代碼可維護性變差、容易出錯。

### 較好的實踐方式 (狀態改變-> 構建新的DOM 元素更新頁面)

這個方法的好處是可以在render方法裡面使用最新的this.state來構造不同HTML結構的字符串，並且通過這個字符串構造不同的DOM元素。

``` js
class LikeButton {
  constructor () {
    this.state = { isLiked: false }
  }

  setState (state) {
    this.state = state
    this.el = this.render()
  }

  changeLikeText () {
    this.setState({
      isLiked: !this.state.isLiked
    })
  }

  render () {
    this.el = createDOMFromString(`
      <button class='like-btn'>
        <span class='like-text'>${this.state.isLiked ? '取消' : '点赞'}</span>
        <span>👍</span>
      </button>
    `)
    this.el.addEventListener('click', this.changeLikeText.bind(this), false)
    return this.el
  }
}
```
* render函數里面的HTML字符串會根據 `this.state` 不同而不同。
* 新增一個 `setState` 函數，這個函數接受一個物件作為參數；它會設置實例的 `state`，然後重新調用一下 `render` 方法。
* 當用戶點擊按鈕的時候，`changeLikeText` 會構建新的 `state` 對象，這個新的`state`，傳入 `setState` 函數當中

因此整個事件觸發的流程就會變成，點擊觸發後 `changeLikeText` 都會調用改變組件狀態然後調用 `setState`，`setState` 會調用 `render`，`render` 方法會根據 `state` 的不同重新構建不同的DOM元素。<font color="#f00">也就是說**只要調用setState，組件就會重新渲染。**</font>

### 重新插入新的 DOM 元素
``` js
setState (state) {
  const oldEl = this.el
  this.state = state
  this.el = this.render()
  if (this.onStateChange) this.onStateChange(oldEl, this.el)
}
```
使用這個component時
``` js
const likeButton = new LikeButton()
wrapper.appendChild(likeButton.render()) // 第一次插入 DOM 元素
likeButton.onStateChange = (oldEl, newEl) => {
  wrapper.insertBefore(newEl, oldEl) // 插入新的元素
  wrapper.removeChild(oldEl) // 删除旧的元素
}
```
實例化以後新增 `onStateChange` 這個方法，可以自定義 `onStateChange` 的功能。這裡做的事是，每當 `setState` 中構造完新的DOM元素以後，就會通過 `onStateChange` 告知外部**插入新的DOM元素，然後刪除舊的元素，頁面就更新了**。
雖然這種方法，每次更新都會重新新增或刪除DOM元素，耗費效能，但可以透過 Virtual-DOM 解決這個問題。

### 參考資料
https://hyf.js.org/react-naive-book (作者：鬍子大哈)