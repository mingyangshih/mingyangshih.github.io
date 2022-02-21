---
title: 100 Days of Code - Instance, State, Higher Order Function
date: 2022-01-16 23:30:40
description: Instance, State, Higher Order Function
categories: python
tags:
  - python
---

### Python Higher Order Function and Event Listener
<font color=#ff0000>Higher Order Function: An function can take another function as a parameter and working inside it.</font>
* Functions as an Inputs, <font color=#ffff00> when function as input only need to pass name.</font>
``` python
<!-- function_a is a higher order function -->
def function_a(something):
  # Do this with something
  # Then do this
  # Final do this

def function_b():
  # Do this

function_a(function_b)
```

```python
from turtle import Turtle, Screen

tim = Turtle()
screen = Screen()


def move_forward():
    tim.forward(10)


# listen screen event, ref: Python Turtle document
screen.listen()
# Press space call move_forward function
screen.onkey(key="space", fun=move_forward)

screen.exitonclick()
```

### Etch-A-Sketch App

```python
# W: Forward, S: Backward, A: Counter-Clockwise, D: Clockwise, C: Clear drawing

from turtle import Turtle, Screen

tim = Turtle()
screen = Screen()


def move_forward():
    tim.forward(10)


def move_backward():
    tim.backward(10)


def turn_left():
    new_heading = tim.heading() + 10
    tim.setheading(new_heading)


def turn_right():
    new_heading = tim.heading() - 10
    tim.setheading(new_heading)

# clear all line and tak turtle to origin
def clear():
    tim.clear()
    tim.penup()
    tim.home()
    tim.pendown()

# listen screen event, ref: Python Turtle document
screen.listen()

# W: Forward, S: Backward, A: Counter-Clockwise, D: Clockwise, C: Clear drawing

screen.onkey(key="w", fun=move_forward)
screen.onkey(key="s", fun=move_backward)
screen.onkey(key="a", fun=turn_left)
screen.onkey(key="d", fun=turn_right)
screen.onkey(key="c", fun=clear)
screen.exitonclick()
```

### Object State and instances

``` python


```