---
title: 用 create-react-app 起一個 React 專案
date: 2021-07-16 22:40:59
description: 用 create-react-app 起一個 React 專案
categories: Frontend
tags:
  - React 
---

在安裝 creact-react-app 前需要先安裝 node.js，可去官網下載即可。

依照 react 官網說明先安裝 creact-react-app 至本機環境
``` js 
npm install -g create-react-app
```

透過 create-react-app 創建一個 react 專案
``` js
create-react-app first-react
```
上方指令會創建一個叫作 first-react 的 react 專案

創建完專案後進入專案資料夾，透過 npm 啟動 react devServer
``` js
// 進入專案資料夾
cd first-react
// 啟動專案 devServer
// 可透過 
npm start
// 或
yarn start 
```