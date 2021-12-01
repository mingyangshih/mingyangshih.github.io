---
title: 100 Days of Code - Object Oriented Programming(OOP) Day16)
date: 2021-11-15 08:58:34
description: Object Oriented Programming(OOP) of Python
categories: python
tags:
  - python
---

### How to use OOP : Classes and Objects
* OOP is trying to model real world object.
* The two most important things that make up an object is attributes and methods.

### EX: Turtle Graphics
* Screen is the turtle where to show up.

``` python
from turtle import Turtle, Screen

timmy = Turtle() # create a Turtle object called timmy

timmy.shape("turtle") # funtion of timmy object which can change shape to a turtle.
timmy.color('red', 'blue') # change the color of the timmy object, red is outline, blue is color of object.

my_screen = Screen()

my_screen.exitonclick() # allow code continue running until we click it

print(my_screen.canvheight) # can get canvheight attribute.

```

### Create Class in Python

```python 

class User:   # Always use PascalCase
  
  def __init__(self, user_id, user_name):  # Will be called everytime creating a object
    # attributes of class
    self.id = user_id
    self.username = user_name
    self.following = 0

  def follow(self): # method of class
    user.followers += 1
    self.following += 1


user_1 = User("001",, "Clayton")
user_2 = User("002",, "Jack")
user_1.follow(user_2)

```