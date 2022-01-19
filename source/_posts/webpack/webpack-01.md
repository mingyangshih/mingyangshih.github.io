---
# 部落格文章的變數
title: Webpack Note 01
date: 2021-06-30
description: Webpack Learning Note
categories: Webpack
tags:
  - Webpack
---

在 Webpack 中一切檔案皆模組，透過 Loader 轉換檔案，透過 Plugin 植入鉤子，最後輸出由多個模組合成的檔案，Webpack 專注於建制模組化專案。

## Install Webpack

### Install Webpack to Project

```javascript
#安裝最新的穩定版本到專案並存在package.json
npm i webpack --save

## 安裝指定版本
npm install webpack@<version>

## 安裝最新的體驗版本
npm install webpack@beta
```

### Install Webpack to Global Enviroment

```javascript
npm install -g webpack
```

- 在專案根目錄下對應的命令列裡面，會透過`node_modules/.bin/webpack`執行 Webpack 的可執行檔
- 在 `NPM Script` 裡定義的工作會優先使用本專案下的 Webpack，寫在 package.json 內容如下：

```json
"script": {"start": "webpack --config webpack.config.js"}
```

### Use Webpack

透過 **Webpack** 建置一個採用 CommonJS(use require)模組化撰寫的專案。

```html
#index.html
<html>
  <head>
    <meta charset="UTF-8" />
  </head>
  <body>
    <div id="app"></div>
    ## 匯入webpack輸出的Javascript檔案
    <script src="./dist/bundle.js"></script>
  </body>
</html>
```

與 index.html 互動的 show.js 內容：

```js
#show.js;
function show(content) {
  window.document.getElementById('app').innerText = 'Hello' + content;
}
module.exports = show;
```

Webpack 執行入口的 main.js 檔案內容：

```js
#main.js;
const show = require('./show.js');
show('Webpack');
```

Webpack 在執行建置時籲社會從專案根目錄下的 webpack.config.js 檔案中讀取設定，內容如下：

```js
const path = require('path');
module.exports = {
  ##JavaScript 執行入口檔案
  entry: './main.js',
  output:{
    ## 將所有依賴的模組合併輸出到一個bundle.js檔案
    filename: 'bundle.js',
    ## 將輸出檔案都放到dist目錄下
    path: path.resolve(__dirname,'./dist')
  }
}
```

```json
{
  "scripts": {
    "start": "webpack --config webpack.config.js"
  }
}
```

在專案目錄下執行`npm start` 就會執行 Webpack 建置，在根目錄下會產生一個 dist 目錄，裡面含有 bundle.js 檔，其內容包含上面所寫的兩個模組，main.js 跟 show.js 及內建的 webpackBootstrap 啟動函數。這時用瀏覽器開啟 index.html 就可以看到 `Hello, Webpack`。

Ｗ ebpack 是一個包裝模組化 JavaScript 的工具，會從 main.js 出發，辨別出原始程式中的模組化匯入敘述，遞迴的找出入口檔案的所有依賴，將入口和其所有依賴包裝到一個單獨的檔案中。Webpack2 開始已經內建對 ES6, CommonJS, AMD 模組化的敘述支援。

## 常見名詞介紹

### loader

Webpack 只能理解 JavaScript 跟 JSON 文件，而 loader 的功能就是為了能讓 Webpack 能去處理其他類型的文件並將他們轉換成有效的模塊(Modules)，以供應用程式使用。

### plugin

loader 用於轉換某些類型的模塊(Modules)，而插件(plugin)則可以用於執行範圍更廣的任務。包括：打包優化，資源管理，注入環境變量。

### mode

通過選擇 development,production 或 none 之中的一個，來設置 mode 參數，你可以啟用 webpack 內置在相應環境下的優化。其默認值為 production。

```js
module.exports = {
  mode: 'production',
};
```
