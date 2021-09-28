---
title: 100 Days of Code - Function Parameters (Day8)
date: 2021-09-14 23:28:45
description: Function Parameters
categories: python
tags:
  - python 
---

### Function Parameters

``` python 
# Function with inputs
# something is called parameter
def my_function(something(parameter)):
  # Do this with something
  print(f"Hello {something}")
# test is called argument, argument is the actual value of the data
my_function('test')
```

### Fcunrion with more than one input

``` python
def greet_with(name, location):
  print(f"Hello, {name}")
  print(f"what is it like in {location}")

greet_with('Clayton','USA')
```

### keyword argument

使用這種方式，就不用考慮傳入 argument 的順序
``` python
def greet_with(name, location):
  print(f"Hello, {name}")
  print(f"what is it like in {location}")

greet_with(location='USA',name='Clayton')
```