---
title: 100 Days of Code - The Complete Python Pro Bootcamp(Day3)
date: 2021-07-13 21:20:24
description: Control Flow and Logical Operators
categories: python
tags:
  - python
---

### if-else

``` python
if condition:
  do this
else:
  do this
```

### Operator and Meaning
``` python 
> : greater than
< : less than
>= : greater than ro equal to
<= : less than or equal to
== : equal to
!= : not equal to
```

### Get remainder 
```python
7%2 # 1

# check even or not 
number = int(input("Which number do you want to check?"))

if number % 2 == 0 :
    print('This is an even number.')
else:
    print('Thisi is an odd number.')
```

### Nested if statements and elif statements

``` python
if condition:
  if another conditino:
    do this
  else: 
    do shit
elif condition2:
  do B
else:
  do this

``` 

### Logical Operators
``` python
A and B
C or D
not E
```

### 補充
* The `lower()` function changes all the letters in a string to lower case.
* The `count()` function will give you the number of times a letter occurs in a string.