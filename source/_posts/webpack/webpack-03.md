---
title: Webpack Note 03 - 使用 Plugin
date: 2021-07-14 22:53:29
description: Webpack Note 03 - 使用 Plugin
categories: Webpack
tags:
  - Webpack
---

### 使用 Plugin

Plugin 是用來擴充 Webpack功能的，透過在建置流程裡實現，可為 Webpack 帶來很大的靈活性。

透過 Plugin 將植入 bundle.js 檔案裡的 CSS 分析到單獨的檔案中，在執行 webpack 前須先安裝 Plugin

```
npm install --save-dev extract-text-webpack-plugin
```

``` js
// webpack.config.js
const path = require('path')
const ExtractTextPlugin = require('extract-text-webpack-plugin')
module.exports = {
  // Webpack 執行檔入口
  entry: './main.js',
  output: {
    // 將所有依賴的模組輸出到 bundle.js 檔案中
    filename: 'bundle.js',
    // 輸出檔案都放到 dist 目錄下
    path: path.resolve(__dirname, './dist')
  },
  module:{
    rules: [
      {
        // 用 regular expression 比對用該 loader 轉換的 CSS 檔案
        test: /\.css$/,
        laders: ExtractTextPlugin.extract({
          // 轉換 .css 檔案需要使用的 Loader
          use: ['css-loader'],
        })
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin({
      // 從 bundle.js 檔案中分析出來的 .css 檔案的名稱
      filename: `[name]_[contenthash:8].css`
    })
  ]
}
```

上方的 code 在練習時會出現error，上網搜尋了一下原因發現 extract-text-webpack-plugin ->  This plugin has been DEPRECATED. 建議改採用 mini-css-extract-plugin，修正如下方，

``` js
const path = require('path')
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports ={
  entry: './main.js',
  output:{
    filename: 'bundle.js',
    path: path.resolve(__dirname,'./dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader'],
      },
    ],
  },
  plugins: [new MiniCssExtractPlugin({
    filename :`[name]_[contenthash:8].css`
  })],
}
```

``` json
{
"name": "webpack_book",
"version": "1.0.0",
"description": "",
"main": "index.js",
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "start": "webpack --mode development --config webpack.config.js",
},
"author": "",
"license": "ISC",
"devDependencies": {
  "css-loader": "^5.2.6",
  "extract-text-webpack-plugin": "^3.0.2", // 已棄用
  "mini-css-extract-plugin": "^2.1.0",
  "style-loader": "^3.0.0",
  "webpack": "^5.40.0",
  "webpack-cli": "^4.7.2"
}
```
執行完成後，可以發現 dist 資料夾下出現一個 [name]_[hash8].css 的檔案，bundle.js內也沒有 CSS 相關內容。

### 小結
Webpack 是透過 plugins 屬性來設定需要使用的外掛程式清單的， plugins 屬性是一個陣列，裡面的每一項都是外掛程式的 instance，在產生 instance 時可以透過建置函數傳入這個元件支援的設定屬性。
如：filename 屬性，可告訴外掛程式 CSS 檔名希望產生的格式。