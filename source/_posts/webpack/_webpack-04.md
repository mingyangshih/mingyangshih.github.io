---
title: Webpack Note 03 - 使用 DevServer
date: 2021-07-15 23:04:59
description: Webpack Note 03 - 使用 Plugin
categories: Webpack
tags:
  -Webpack
---
## html-webpack-plugin 自動把檔案匯入到html
打包整個專案時，透過需要將打包出來的 CSS, JS 檔加到 html檔內，若檔案變多時就不太可能透過人工去匯入，因此可以透過 `html-webpack-plug` 這個 plugin 讓檔案自動加到 html 檔案中。

``` js
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports ={
  entry: './main.js',
  output:{
    filename: 'bundle.[hash].js',
    path: path.resolve(__dirname,'./dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader'],
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: ['babel-loader']
      }
    ],
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename :`[name]_[contenthash:8].css`
    }),
    new HtmlWebpackPlugin({
      template:'index.html'
    })
  ],
}
```


## 使用 DevServer

在實際開發中會需要
* 提供 HTTP or HTTPS服務而不是使用靜態檔案預覽
* 監聽檔案的變化並自動更新網頁，做到即時預覽。
* 支援 Source Map，以方便 Debug。

Webpack 原生支援上述第二第三點，再結合官方提供的開發工具 DevServer 也可以很方便做到第一點。

整合 DevServer 近專案前，須先透過下方指令安裝
`npm install -D webpack-dev-server` 