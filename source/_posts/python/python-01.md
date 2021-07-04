---
title: 100 Days of Code - The Complete Python Pro Bootcamp(Day1)
date: 2021-07-03
description: Working With Variables in Python to Manage Data
categories: python
tags: 
  - python
---

## Python input function
``` python
print("Hello" + input("What's your name."))
```

執行上方程式碼時，`input("What's your name.")` 會被替換成輸入的值，並和Hello組成字串，print出來。可透過Thonny 這個開發 IDE 一個步驟一個步驟看。

## python 兩個變數對調方法

### 方法一(create a temporary variable and swap the values)

``` python 
a = input("a: ")
b = input("b: ")

＃透過第三個變數存放a || b其中一個變數
c = a
a = b
b = c

print("a: " + a)
print("b: " + b)
```

### 方法二(Without Using Temporary Variable)

``` python
#使用python 變數對調語法
a = input("a: ")
b = input("b: ")

a,b = b,a

print("a: " + a)
print("b: " + b)
```