---
title: 100 Days of Code - For Loops, Range and Code Blocks (Day5)
date: 2021-08-03 22:55:18
description: Python Loops
categories: python
tags:
  - python
---

### for loop

``` python 
for item in list_or_items:
  #do something to each item

# eg1
student_heights = input("Input a list of student heights ").split()
for n in range(0, len(student_heights)):
  student_heights[n] = int(student_heights[n])

total_height = 0
totalperson = 0
for height in student_heights:
  total_height += height
  totalperson += 1

average = round(total_height/totalperson)
print(average)

# Find max value in list
student_scores = input("Input a list of student scores ").split()
for n in range(0, len(student_scores)):
  student_scores[n] = int(student_scores[n])
print(student_scores)

max = 0
for x in student_scores :
  if (x > max):
    max = x

print(max)

```

### for loop and range() function

```python
# a to b, not include b, increase by c
for number in range(a,b,c):
  print(number)

#eg1
sum = 0
for i in range(2,101,2):
  sum += i

sum = sum + 1
print(sum)

# 重新隨機編排一個 list
x = [1,2,3,4,5]
random.shuffle(x)
```
