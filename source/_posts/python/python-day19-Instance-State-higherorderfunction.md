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
Create multiple objects by a class, and their functions are totally independent to each other.
So, we can say they're seperately instance. They can have different attribute.

``` python
# tim is an object, Turtle is a class.
tim = Turtle()
# we can create more object by class
tom = Tuttle()
```

### Understanding the Turtle Coordinate System.

Turtle coordinate center is `(0,0)`.

``` python
from turtle import Turtle, Screen

screen = Screen()
# setting screen size
screen.setup(width=500, height=400)
# create popup to input text
user_bet = screen.textinput(title="Make your bet", prompt="Which turtle will win the race? Enter a color: ")
colors = ["red", "orange", "yellow", "green", "blue", "purple"]
first_turtle_y_position = -100

for item in range(len(colors)):
    # set turtle starting positoin and color
    tim = Turtle(shape="turtle")
    tim.color(colors[item])
    tim.penup()
    tim.goto(x=-230, y=first_turtle_y_position)
    first_turtle_y_position += 30

screen.exitonclick()
```

### Final code

``` python
import turtle
from turtle import Turtle, Screen
import random

is_race_on = False
screen = Screen()
# setting screen size
screen.setup(width=500, height=400)
# create popup to input text
user_bet = screen.textinput(title="Make your bet", prompt="Which turtle will win the race? Enter a color: ")
colors = ["red", "orange", "yellow", "green", "blue", "purple"]
first_turtle_y_position = -100
all_turtles = []

for item in range(len(colors)):
    # set turtle starting positoin and color
    new_turtle = Turtle(shape="turtle")
    new_turtle.color(colors[item])
    new_turtle.penup()
    new_turtle.goto(x=-230, y=first_turtle_y_position)
    # append to all_turtles list
    all_turtles.append(new_turtle)
    first_turtle_y_position += 30

if user_bet:
    is_race_on = True

while is_race_on:
    for turtle in all_turtles:
        # check if any turtle reach the right bound
        if turtle.xcor() > 230:
            is_race_on = False
            winning_color = turtle.pencolor()
            if winning_color == user_bet:
                print(f"You won! The {winning_color} turtle win.")
            else:
                print(f"You lost! The {winning_color} turtle win.")
        rand_distance = random.randint(0, 10)
        turtle.forward(rand_distance)

screen.exitonclick()

```