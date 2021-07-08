---
title: 100 Days of Code - The Complete Python Pro Bootcamp(Day2)
date: 2021-07-05
description: Understanding Data Types and How To Manipulate Strings
categories: python
tags: 
  - python
---

## Python Primitive Data Types

### String

``` python
'Hello'[0] #Can get a first character of a string
#This method of pulling out a particular element from a string is called sub-scripting.
print('Hello'[len('Hello')-1])
print('123' + '345') # 123345
```

### Integer
Number data type one of the most common that you'll see is called an integer.

``` python
123 + 345 # 468
print(123_456_789) # we can replace big numbers commas simply with underscores 
# and it will be interpreted by the computer as if you had written this. 
# So the computer actually removes those underscores and ignores it.
```

### Float
Decimal number they are called a float and this is short for a floating-point number.
``` python
3.14159

# string to float bmi practice
weight = input('weight')
height = input('height')

bmi = float(weight) / float(height) / float(height)

print(bmi)

```

### Boolean
Only two possible value true or false
``` python 
True
False
```

## Type Error, Type Checking and Type Conversion

``` python
num_char = len(input("What's your name?"));
print("your name has" + num_char + "characters."); # TypeError: can only concatenate str (not "int") to str

type(num_char) # use this to see type of this variable

# type conversion change data from a int to string
new_num_char = str(num_char)
print("your name has" + new_num_char + "characters.")
```

### exercise 2.1 
write a program that adds the digits in a 2 digit number. e.g. if the input was 35, then the output should be 3 + 5 = 8

``` python 

two_digit_number = input("Type a two digit number: ")
print(int(two_digit_number[0]) + int(two_digit_number[1]))

```

## Mathematical Operations in Python

運算優先順序：PEMDAS(Parentheses, Exponents, Multiplication, Division, Addition, Subtration)
如相同等級如，乘除相同等級，由左到右依序計算。

``` python
3 + 5

7 - 4

3 * 8

type(6 / 3) # float because the answer is 2.0 

2**2  # get a exponent number to a power
```

## Number Manipulation and F Strings in Python

``` python

round(8/3, 2) # 四捨五入，第二個參數可以設定到第幾位

#floor division 若只想取整數可用下方這樣寫
print(8//3) # 用type 檢查他會是一個int

score = score + 1 # 可寫成
scoer += 1 ## -= *= /= 同理

```

## f-string 不用自己做型別轉換串起字串
``` python
score = 0
height = 1.8
isWinning = True

f"Hello Clayton you score is {score}" 

```