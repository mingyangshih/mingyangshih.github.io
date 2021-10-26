---
title: 100 Days of Code - Scopes and Number (Day12)
date: 2021-10-24 22:44:34
description: Scopes and Number
categories: python
tags:
  -python
---

### Namespaces : Local vs Global Scope

``` python
enemies = 1

def increase_enemies():
  enemies = 2
  print(f"enemies inside a function {enemies}")
increase_enemies()

print(f"enemies outside a function {enemies}")

```

### Local Scope
Local Scope exists within functions.

``` python
def increase_enemies():
  enemies = 2
  print(f"enemies inside a function {enemies}")
increase_enemies()
print(enemies) # enemies is not defined

```

### Does Python have block scope
There is not such thing as block scope in Python.

``` python
if true :
  new_enemy = 'Skeleton'

print(new_enemy) # 這樣是可以的，因為沒有 block scope

```

### How to modify a Global Variable

``` python 
enemies = 1

def increase_enemies():
  global enemies # takes global enemies into the function.
  enemies += 1
  print(f"enemies inside a function {enemies}")

increase_enemies()
print(enemies)
```

### Python Constants and Global Scope

Global constants are variables which you define and you never planning on changing it ever again. 
Usually using uppercase to represent it.
