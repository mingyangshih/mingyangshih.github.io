---
title: 100 Days of Code - Randomisation and Python List (Day4)
date: 2021-07-26 23:10:32
description: Randomisation and Python List
categories: python
tags:
  - python
---

### Mersenne Twister (python 產生隨機數字的方法) （Pseudorandom number)
[參考連結](https://www.khanacademy.org/computing/computer-science/cryptography/crypt/v/random-vs-pseudorandom-number-generators)

* 使用 random module 產生隨機變數，可去 [askpython](https://www.askpython.com/)，看使用方式。

``` python
import random

a = random.randint(1, 10)
print(a)

b = random.random() # 產生 [0,1) 的隨機浮點數。
print(b)

# 從 list 中隨機選出一元素
x = [1,2,3,4,5]
random.choice(x)

# 重新隨機編排一個 list
x = [1,2,3,4,5]
random.shuffle(x)
```

* module 是什麼：一個.py檔可視為一個 module

### List

``` python 
fruits = ['cherry','apple','pear']
fruits[0] # cherry

# 負數 從後面開始找起
fruits[-1] # pear

# 在最後面加資料
fruits.append('banana')

# random.choice(list)可從list中隨機選出一參數
random.choice(fruist)

# fruits.index('cherry') 可找出某個值在 list 中的索引值
fruits.index('cherry')
```