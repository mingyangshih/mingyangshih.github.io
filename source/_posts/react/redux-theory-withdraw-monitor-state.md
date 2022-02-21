---
title: Introduction of Redux withdraw and monitor state
date: 2022-01-19 22:17:51
categories: Frontend
description: Introduction of Redux withdraw and monitor state
tags:
  - React
---

## Redux : 抽離 store 和監控數據變化

### 抽離出 store
把 `appState` 跟 `dispatch` 集中到 `store` 下做管理，然後建立一個 `createStore` 用來產生 `state` 跟 `dispatch` 的集合。

```js
let appState = {
  title: {
    text: 'React.js 小书',
    color: 'red',
  },
  content: {
    text: 'React.js 小书内容',
    color: 'blue'
  }
}

function dispatch (action) {
  switch (action.type) {
    case 'UPDATE_TITLE_TEXT':
      appState.title.text = action.text
      break
    case 'UPDATE_TITLE_COLOR':
      appState.title.color = action.color
      break
    default:
      break
  }
}

function createStore (state, stateChanger) {
  const getState = () => state
  const dispatch = (action) => stateChanger(state, action)
  return { getState, dispatch }
}

function renderApp (appState) {
  renderTitle(appState.title)
  renderContent(appState.content)
}

function renderTitle (title) {
  const titleDOM = document.getElementById('title')
  titleDOM.innerHTML = title.text
  titleDOM.style.color = title.color
}

function renderContent (content) {
  const contentDOM = document.getElementById('content')
  contentDOM.innerHTML = content.text
  contentDOM.style.color = content.color
}
```

`createStore` 接收兩個參數，一個是表示application的 `state` ，另一個就是 `stateChanger` ，他用來描述 application 狀態會跟著 action 發生什麼變化。
`createStore` 會回傳一個物件，這個物件包含兩個方法 `getState` 和 `dispatch`。`getState` 用來取得 `state` 數據，其實就是簡單的回傳 `state`。
`dispatch` 用來修改數據，接受 `action`，然后把 `state` 和 `action` 一起傳給 `stateChanger`，`stateChanger` 就可以根據 `action` 來修改 `state` 了。

修改產生數據的方式：
```js
const store = createStore(appState, stateChanger)

renderApp(store.getState()) // 首次渲染頁面
store.dispatch({ type: 'UPDATE_TITLE_TEXT', text: '《React.js 小书》' }) // 修改title
store.dispatch({ type: 'UPDATE_TITLE_COLOR', color: 'blue' }) // 修改title顏色
renderApp(store.getState()) // 渲染新的資料狀態

```
針對不同的 Application 可以給 `createStore` 傳入初始的 `appState`，和一描述狀態變化的函數 `stateChanger`，然後產生一個 `store`。要修改資料的时候透過 `store.dispatch`，需要取得資料的時候透過 `store.getState`。


### 監控數據變化

上面的 `code` 有個問題，每次透過 `dispatch` 改數據時，其實只是 `state` 發生變化，如果不主動 call `renderApp`，頁面上的內容是不會發生變化的。
如何在數據變化的時候，能夠自動重新渲染畫面，而不是主動呼叫 `renderApp` :
在 `createStore` 的 `dispatch` 內加入 `renderApp` 即可，這裡會使用到訂閱者模式來監聽數據變化。

``` js
function createStore (state, stateChanger) {
  const listeners = []
  const subscribe = (listener) => listeners.push(listener)
  const getState = () => state
  const dispatch = (action) => {
    stateChanger(state, action)
    listeners.forEach((listener) => listener())
  }
  return { getState, dispatch, subscribe }
}
```

在 `createStore` 內定義一個 `array` `listeners` ， 還有一個新方法 `subscribe` 可以透過 `store.scribe(listener)` 的方式給 `subscribe` 傳入一個監聽函數，這個函數會 `push` 到 `listeners` 中。
此外，也修改了 `dispatch` 每次當他被使用時，除了會呼叫 `stateChanger` 修改數據，還會遍歷 `listeners` `array`內的函數，並一個一個呼叫他；相當於我們可以透過 `subscribe` 傳入數據變化的監聽函數，每當 `dispatch` 的時候，監聽和樹就會被呼叫，這樣就可以在數據變化時進行重新渲染：
只需要 `subscribe` 一次，後面不管如何 `dispatch` 進行修改數據， `renderApp` 函數都會被重新呼叫，頁面就會被重新渲染，這樣的訂閱模式還有好處就是，以後我們還可以拿同一塊數據來渲染別的頁面，此時 `dispatch` 導致的變化也會讓每個頁面都重新渲染。

### 完整代碼
```js  
function createStore (state, stateChanger) {
  const listeners = []
  const subscribe = (listener) => listeners.push(listener)
  const getState = () => state
  const dispatch = (action) => {
    stateChanger(state, action)
    listeners.forEach((listener) => listener())
  }
  return { getState, dispatch, subscribe }
}

function renderApp (appState) {
  renderTitle(appState.title)
  renderContent(appState.content)
}

function renderTitle (title) {
  const titleDOM = document.getElementById('title')
  titleDOM.innerHTML = title.text
  titleDOM.style.color = title.color
}

function renderContent (content) {
  const contentDOM = document.getElementById('content')
  contentDOM.innerHTML = content.text
  contentDOM.style.color = content.color
}

let appState = {
  title: {
    text: 'React.js book',
    color: 'red',
  },
  content: {
    text: 'React.js book content',
    color: 'blue'
  }
}

function stateChanger (state, action) {
  switch (action.type) {
    case 'UPDATE_TITLE_TEXT':
      state.title.text = action.text
      break
    case 'UPDATE_TITLE_COLOR':
      state.title.color = action.color
      break
    default:
      break
  }
}

const store = createStore(appState, stateChanger)
store.subscribe(() => renderApp(store.getState())) // listen state change

renderApp(store.getState()) // Render first time
store.dispatch({ type: 'UPDATE_TITLE_TEXT', text: '《React.js》' }) 
store.dispatch({ type: 'UPDATE_TITLE_COLOR', color: 'blue' }) 
```