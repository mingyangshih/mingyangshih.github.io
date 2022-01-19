---
title: Introduction of Redux
date: 2021-12-23 08:22:33
categories: Frontend
description: Introduction of Redux
tags:
  - React
---

### What is Redux

Redux 和 React-redux 不是同一個東西。Redux 是一種架構模式（Flux 架構的一種變形)。React-redux 就是把 Redux 架構模式與 React.js 結合的一個 Library，就是 Redux 架構 React.js 中的實現。

```html
<body>
  <div id="title"></div>
  <div id="content"></div>
</body>
```

```js
const appState = {
  title: {
    text: 'React.js 小书',
    color: 'red',
  },
  content: {
    text: 'React.js 小书内容',
    color: 'blue',
  },
};

function renderApp(appState) {
  renderTitle(appState.title);
  renderContent(appState.content);
}

function renderTitle(title) {
  const titleDOM = document.getElementById('title');
  titleDOM.innerHTML = title.text;
  titleDOM.style.color = title.color;
}

function renderContent(content) {
  const contentDOM = document.getElementById('content');
  contentDOM.innerHTML = content.text;
  contentDOM.style.color = content.color;
}
// renderApp 會呼叫 rendeTitle 和 renderContent，而這兩個函式會把 appState 裡面的數據通過原始的 DOM 操作更新到頁面上
renderApp(appState);
```

上方是一個很簡單 App，但是它存在一個嚴重的隱憂，渲染資料時，使用的是一个共享狀態的 `appState`，每個人都可以對他進行操作。
但不同的 `component` 之間又需要共享資料，這些 `component` 可能也需要修改共享資料，因此會產生矛盾就是：`component` 之間需要共享資料 和 資料可能被隨意修改而導致無法預期的結果之間的矛盾。

React.js 團隊的做法是：`component` 之間可以共享資料，也可以改資料。但是這個資料不能直接改，<b>只能透過執行 `dispatch` 這個專門修改資料的 `function`</b>。

```js
function dispatch(action) {
  switch (action.type) {
    case 'UPDATE_TITLE_TEXT':
      appState.title.text = action.text;
      break;
    case 'UPDATE_TITLE_COLOR':
      appState.title.color = action.color;
      break;
    default:
      break;
  }
}
```

所有對資料的操作必須經過 `dispatch` 函數。他接收一個參數 `action`，这个 `action` 是一個普通的 JS 物件(object)，裡面必須包含一個 `type` 屬性聲明想做什麼事。`dispatch` 在 `swtich` 內判斷 `type` 參數，符合條件的 `type` 參數才會執行對 `appState` 的修改。

``` js
let appState = {
  title: {
    text: 'React.js title',
    color: 'red',
  },
  content: {
    text: 'React.js content',
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

renderApp(appState) // first render
dispatch({ type: 'UPDATE_TITLE_TEXT', text: '《React.js book》' }) //change title
dispatch({ type: 'UPDATE_TITLE_COLOR', color: 'blue' }) //change color
renderApp(appState) //second render
```
