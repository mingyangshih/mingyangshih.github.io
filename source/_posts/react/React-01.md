---
title: React小書 Note 01
date: 2021-07-03
description: React小書 Learning Note (前端組件化一)
categories: Frontend
tags: 
  - React
  - JS Framework
---

## 原生JS 實現簡單的按鈕組件化
``` js
// ::String => ::Document
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