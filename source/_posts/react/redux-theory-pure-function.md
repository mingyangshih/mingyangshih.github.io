---
title: Introduction of Redux - Pure Function
date: 2022-02-09 22:56:23
categories: Frontend
description: Introduction of Redux - Pure Function
tags:
  - React
---

## Pure Function

一個函數的回傳結果只依賴於它的參數，並且在執行過程中沒有副作用，就可以把這個函數稱作 `Pure Function`。

所以可以歸納為：

1. <font color=#ffff00>函數的回傳結果只依賴於他的參數。</font>
2. <font color=#ffff00>函數執行過程中沒有副作用。</font>


### 函數的回傳結果只依賴於他的參數

``` js 
const a = 1
const test = (b) => a + b
test(2) // => 3
```
`test` 函數不是一個纯函數，因为回傳的結果依賴外部變數 `a`，在不知道 `a` 的情況下，不能保證 `test(2)` 的回傳職是 `3`。雖然 `test` 函數的回傳值並沒有變化，傳入的參數也沒有變化，但他回傳的值是不可預料的。

``` js
const a = 1
const test = (x, b) => x + b
test(1, 2) // => 3
```
現在 `test` 的回傳結果只依賴於參數 `x` 和 `b`，`test(1, 2)` 永遠是 `3`。不管外部發生什麼變化，`test(1, 2)` 永遠是 `3`。只要 `test` code 不變，傳入的參數是確定的，那麼 `test(1, 2)` 的值永遠是可以預料的。

這就是 `Pure Function` 的第一個條件： <font color=#ffff00>**一個函數的返回結果只依賴於他的參數**</font>

### 函數執行過程沒有副作用

一個函數執行時產生<font color=#ffff00>**對外部可觀察的變化**</font>即可稱這函數有副作用。

``` js
const a = 1
const test = (obj, b) => {
  return obj.x + b
}
const counter = { x: 1 }
test(counter, 2) // => 3
counter.x // => 1
```
上方 code 可以傳一個物件進行計算，計算過程裡面並不會對傳入的物件進行修改，計算前後的 `counter` 不會發生任何變化，計算前是 `1`，計算後也是 `1` ，所以現在是屬於 `Pure Function`。

``` js
const a = 1
const test = (obj, b) => {
  obj.x = 2
  return obj.x + b
}
const counter = { x: 1 }
test(counter, 2) // => 4
counter.x // => 2
```
修改一下 code 在 `test` 內部加上 `obj.x=2` 此步驟會修改到傳進 `test` 的 物件，`counter.x` 計算後會變成 `2`，因此函數執行時對外部產生了影響，即產生了<font color=#ffff00>**副作用**</font>，因為他修改了外部傳進來的物件，所以<font color=#ffff00>**不是**</font> `Pure Function`。

但在函數內部創建的變數，進行修改不是副作用：

``` js
const test = (b) => {
  const obj = { x: 1 }
  obj.x = 2
  return obj.x + b
}
```

除了修改外部變數之外，函數在執行過程中還有很多方式產生<font color=#ffff00>外部可觀察的變化</font>，比如說使用 `DOM API` 修改頁面，或者發送 `AJAX` 請求，使用 `window.reload` 重新整理，甚至是 `console.log` 等都是副作用。
因此 `Pure Function` 的定義很嚴格，除了計算數據之外什麼都不能做，計算時還不能依賴函數參數以外的數據。

### 小結
一個函數回傳的結果之依賴於他的參數，並且在執行過程內沒有副作用，我們就把這個函數稱為 `Pure Function` 。 `Pure Function` 不會產生不可預料的行為，也不會對外部產生影響，給他什麼就會吐出相對應的資料，若應用程式大多數函式都是 `Pure Function` 那麼程序測試、調校起來會較方便。