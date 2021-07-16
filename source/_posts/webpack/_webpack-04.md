---
title: Webpack Note 03 - 使用 DevServer
date: 2021-07-15 23:04:59
description: Webpack Note 03 - 使用 Plugin
categories: Webpack
tags:
  -Webpack
---
## html-webpack-plugin 自動把檔案匯入到html
## 使用 DevServer

在實際開發中會需要
* 提供 HTTP or HTTPS服務而不是使用靜態檔案預覽
* 監聽檔案的變化並自動更新網頁，做到即時預覽。
* 支援 Source Map，以方便 Debug。

Webpack 原生支援上述第二第三點，再結合官方提供的開發工具 DevServer 也可以很方便做到第一點。

整合 DevServer 近專案前，須先透過下方指令安裝
`npm install -D webpack-dev-server` 