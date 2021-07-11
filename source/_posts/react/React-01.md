---
title: React小書 Note 01
date: 2021-07-03
description: React小書 Learning Note (前端組件化一) 從一個簡單的例子講起
categories: Frontend
tags: 
  - React
---
React 核心觀念，state 永遠會對應到一個UI;改變state 時會trigger re-render

## 原生JS 實現簡單的按鈕組件化
``` js
const wrapper = document.querySelector('.wrapper')
const likeButton1 = new LikeButton()
wrapper.appendChild(likeButton1.render())

const likeButton2 = new LikeButton()
wrapper.appendChild(likeButton2.render())

// ::String => ::Document Object Model
const createDOMFromString = (domString) => {
  const div = document.createElement('div')
  div.innerHTML = domString
  return div
}

class LikeButton {
  constructor () {
    this.state = { isLiked: false }
  }

  changeLikeText () {
    const likeText = this.el.querySelector('.like-text')
    this.state.isLiked = !this.state.isLiked
    likeText.innerHTML = this.state.isLiked ? '取消' : '点赞'
  }

  render () {
    this.el = createDOMFromString(`
      <button class='like-button'>
        <span class='like-text'>点赞</span>
        <span>👍</span>
      </button>
    `)
    this.el.addEventListener('click', this.changeLikeText.bind(this), false)
    return this.el
  }
}

```
### 參考資料

https://hyf.js.org/react-naive-book