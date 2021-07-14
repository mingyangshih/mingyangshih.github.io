---
title: Webpack Note 02 - 使用 Loader
date: 2021-07-14 22:05:35
description: Webpack Note 02 - 使用 Loader
categories: Webpack
tags:
  - Webpack
---

### 使用 Loader

Webpack 將所以檔案看作模組，CSS 檔案也不例外，要引用css 檔，則需要像引用JS檔一樣，修改入口檔案 `msin.ja`。

``` js 
// main.js

// CommonJS標準匯入 CSS模組
require('/main.css');

//CommonJS標準匯入 JS函數 
const show = require('./show.js)

// 執行函數
show('Webpack')
```
因為Webpack 原生不支援解析非JS檔案，所以需要使用Webpack Loader，因此需要將Webpack的設定做修改。
Loader 具有檔案compile的功能，module.rules 陣列設定了一組規則，告訴Webpack在遇到哪些檔案時，要用哪些Loader去compule。
* use 屬性值，需要一個由Loader名稱組成的陣列，<font color="yellow" >Loader執行的順序是由後到前的。</font>
* 每個Loader都可以透過 URL querystring 的方式導入參數，若想知道 Loader 可以支援哪個屬性可以去查各個 Loader 的文件。
* 在執行 Webpack 前 須先安裝有用到的 Loader

如: `npm i -D style-loader css-loader --save`

``` js
const path = require('path');
module.exports = {
  // Webpack 執行入口檔案
  entry:'./main.js',
  outpyt: { // output 設定
    // 將所有依賴模組合併輸出到 bundle.js檔案中
    filename: 'bundle.js',
    path:path.resolve(__dirname,'./dist')
  },
  module:{
    rules: [
      {
        // 用regular expression 去比對要用Loader 轉換的CSS檔案
        test: /\.css$/,
        use :['style-loader','css-loader?minimize']
      }
    ]
  }
}
```

執行 Webpack 後會發現CSS 被寫道 JS 中，因為中間透過 style-loader 處理的結果。
Loader 傳入屬性的方法除了，querystring 外，也可以透過 Object 實現

``` js
use:[
  'style-loader',
  {
    loader: 'css-loader',
    options: {
      minimize: true
    }
  }
]
```
除了在 `webpack.config.js` 設定檔中設定 Loader，還可以在原始程式中（main.js）指定用什麼 Loader去處理檔案。

``` js 
// main.js
require('style-loader!css-leader!.main.css')
```
這樣就可跟上方有一樣的效果。