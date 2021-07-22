---
title: Webpack Note 05 入門核心概念小整理（Entry, MOdule, Chunk, Loder, Plugin, Output）
date: 2021-07-22 22:14:37
description: Webpack Note 05 入門核心概念小整理（Entry, MOdule, Chunk, Loder, Plugin, Output）
categories: Webpack
tags:
  - Webpack
---

* Entry : Webpack 執行建制的第一步就是從 Entry 開始。
* Module : 模組，在 Webpack 內所有東西都是模組，一個模組對應一個檔案，Webpack 會從設定的 Entry 開始遞回找出所有依賴的模組。
* Chunk : 程式區塊，一個 Chunk 由多個模組組合而成，用於程式合併與分割。
* Loader : 模組轉換器，用於將模組的原內容按照需求轉換成新內容。
* Plugin : 擴充外掛程式，在 Webpack 建置流程中的特定時機植入擴充邏輯，來改變建置結果，或想要做的事情。
* Output: 輸出結果，在 Webpack 經過一系列處理並得出最後想要的程式後，輸出結果。

Ｗebpack 啟動後會從 Entry 裡設定的 Module 開始，遞回解析依賴的所有 Module。美找到一個，就會根據設定的 Lodaer 去找出對應的轉換歸則，對 Module進行轉換後，在解析目前 Module 依賴的 Module。這些 Module 會以 Entry 為單位進行分組，一個 Entry 集齊所有依賴的 Module 被分到一個組也就是一個 Chunk。最後 Webpack 會將所有 Chunk 轉換成檔案輸出。在整個流程中， Webpack 會在恰當的時機執行 Plugin 裡定義的邏輯。