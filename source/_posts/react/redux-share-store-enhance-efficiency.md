---
title: Introduction of Redux Share Store to Enhance Efficiency
date: 2022-03-01 10:35:13
categories: Frontend
description: Introduction of Redux Share Store to Enhance Efficiency
tags:
  - React
---
## 提高共享結構的性能

從之前的 <font color=#ffff00>抽離Store和監控數據變化</font> 範例 code 中會遇到一些性能問題；就算不是更新 `state` 中的某個數據， `component` 畫面也會重新渲染。

解決方法：在渲染函數執行前做判斷，判斷傳入的新數據和舊數據是否相同，不同的話就不執行渲染。

``` js
function renderApp (newAppState, oldAppState = {}) {
  if (newAppState === oldAppState) return // 數據沒有變化就不render了
  console.log('render app...')
  renderTitle(newAppState.title, oldAppState.title)
  renderContent(newAppState.content, oldAppState.content)
}

function renderTitle (newTitle, oldTitle = {}) {
  if (newTitle === oldTitle) return // 數據沒有變化就不render了
  console.log('render title...')
  const titleDOM = document.getElementById('title')
  titleDOM.innerHTML = newTitle.text
  titleDOM.style.color = newTitle.color
}

function renderContent (newContent, oldContent = {}) {
  if (newContent === oldContent) return // 數據沒有變化就不render了
  console.log('render content...')
  const contentDOM = document.getElementById('content')
  contentDOM.innerHTML = newContent.text
  contentDOM.style.color = newContent.color
}
```
用一個 `oldState` 變數保存舊的 `state`，在需要重新渲染的時候把新舊數據傳進去：

``` js
const store = createStore(appState, stateChanger)
let oldState = store.getState() // 緩存舊的 state
store.subscribe(() => {
  const newState = store.getState() // 數據可能變化獲取新的 state
  renderApp(newState, oldState) // 把新舊數據傳進去渲染
  oldState = newState // 渲染後，新的 newState 變成舊的 oldState，等待下次數據變化再重新渲染
})
```
透過上面這個方法 `newContent === oldContent` 永遠都會是 `true` 因為物件是 Pass By Reference

### ES6 語法介紹

``` js 
const obj = { a: 1, b: 2}
const obj2 = { ...obj }
```

`const obj2 = { ...obj }` 建立一個新的物件 `obj2`，然後把 `obj` 所有屬性都複製到 `obj2` 裡面，類似於物件的淺複製。除了淺複製還可以覆蓋、解構物件屬性：

``` js
const obj = { a: 1, b: 2 }
// b被新的值取代，並新增c新的值
const obj2 = { ...obj, b: 3, c: 4 } // => { a: 1, b: 3, c: 4} 
```

我們可以透過這種方法應用在 `state` 的更新上，一但我們要修改某些資料，就把修改路徑上的所有物件複製一遍，如：
`appState` 和 `newAppState` 其實是兩個不同的物件，因為物件淺複製的原因，其實內部 `content` 指向的是同一個物件，但是因為 `title` 被一個新的物件覆蓋了，所以他們的 `title` 屬性指向的物件是不同的：
``` js
let newAppState = { // 新建一個 newAppState
  ...appState, // 複製 appState 內的內容
  title: { // 用一個新的物件覆蓋原本title屬性
    ...appState.title, // 複製原來title物件裡面的內容
    text: '《React.js book》' // 覆蓋text屬性
  }
}
// 透過淺複製方法可以達到下方條件，提供判斷資料是否有改變並更新
appState !== newAppState // true
appState.title !== newAppState.title // true
appState.content !== appState.content // false
```
### 優化性能
修改 `stateChanger` 使其不會直接修改原來的數據 `state` ，而是感生上述的共享結構的物件：

``` js
function stateChanger (state, action) {
  switch (action.type) {
    case 'UPDATE_TITLE_TEXT':
      return { // 創建新的物件並回傳
        ...state,
        title: {
          ...state.title,
          text: action.text
        }
      }
    case 'UPDATE_TITLE_COLOR':
      return {
        ...state,
        title: {
          ...state.title,
          color: action.color
        }
      }
    default:
      return state
  }
}
```

### 完整代碼

``` js
function createStore (state, stateChanger) {
  const listeners = []
  const subscribe = (listener) => listeners.push(listener)
  const getState = () => state
  const dispatch = (action) => {
    state = stateChanger(state, action) // 覆蓋原物件
    listeners.forEach((listener) => listener())
  }
  return { getState, dispatch, subscribe }
}

function renderApp (newAppState, oldAppState = {}) { // 預設參數 oldAppState = {}
  if (newAppState === oldAppState) return // 數據沒變化就不重新渲染
  console.log('render app...')
  renderTitle(newAppState.title, oldAppState.title)
  renderContent(newAppState.content, oldAppState.content)
}

function renderTitle (newTitle, oldTitle = {}) {
  if (newTitle === oldTitle) return
  console.log('render title...')
  const titleDOM = document.getElementById('title')
  titleDOM.innerHTML = newTitle.text
  titleDOM.style.color = newTitle.color
}

function renderContent (newContent, oldContent = {}) {
  if (newContent === oldContent) return
  console.log('render content...')
  const contentDOM = document.getElementById('content')
  contentDOM.innerHTML = newContent.text
  contentDOM.style.color = newContent.color
}

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

function stateChanger (state, action) {
  switch (action.type) {
    case 'UPDATE_TITLE_TEXT':
      return { // 創建新的物件並回傳
        ...state,
        title: {
          ...state.title,
          text: action.text
        }
      }
    case 'UPDATE_TITLE_COLOR':
      return { // 創建新的物件並回傳
        ...state,
        title: {
          ...state.title,
          color: action.color
        }
      }
    default:
      return state
  }
}

const store = createStore(appState, stateChanger)
let oldState = store.getState() // 暫存舊的 state
store.subscribe(() => {
  const newState = store.getState() // 數據可能變化，獲取新的 state
  renderApp(newState, oldState) // 把新舊 state 傳進去判斷是否要渲染
  oldState = newState // 渲染完之後，新的 newState 變成舊的 oldState等待下一次更新數據
})

renderApp(store.getState()) // 首次渲染畫面
store.dispatch({ type: 'UPDATE_TITLE_TEXT', text: '《React.js book》' }) // 修改標題文字
store.dispatch({ type: 'UPDATE_TITLE_COLOR', color: 'blue' }) // 修改標題顏色
```