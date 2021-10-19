---
title: 100 Days of Code - Functions with Outputs (Day9)
date: 2021-10-06 23:14:30
description: Functions with Outputs
categories: python
tags:
  -python
---

### Functions with Outputs

``` python
def ex_function() :
  result = 'function result'
  return result

output = ex_function() 

# ex1 

def format_name(f_name, l_name):
  formated_f_name = f_name.title()
  formated_l_name = l_name.title()
  return f"{formated_f_name} {formated_l_name}"

formated_string = format_name('Clayton', 'Shih')

# ex2
def add(num1, num2):
  return num1 + num2
operations = {
  "+" : add
}

calculation_function = operations[operation_symbol]
calculation_function(1,2)
```

### Doctrings

Docstrings are basically a way for us to create little bits of documentation as
we're coding along in our functions or in our other blocks of code.

``` python
def ex_function() :
  """ Docstring example """  # 一定要寫在第一行，在用這function時會看到 Docstring 的提示
  result = 'function result'
  return resule
```


