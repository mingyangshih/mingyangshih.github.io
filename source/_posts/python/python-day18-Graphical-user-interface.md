---
title: 100 Days of Code - Graphical User Interface(Day18)
date: 2021-12-01 22:05:09
description: Turtle & Graphical User Interface
categories: python
tags:
  - python
---


### Importing Modules
``` python
import turtle # import module_name

tim = turtle.Turtle()

from turtle import Turtle # from module_name import thing_in_module

tim = Turtle()

from turtle import * # import everything in module_name(not recommended)

import turtle as t # alias module

```




### Turtle Module Practice

``` python
from turtle import Turtle, Screen

# create a turtle object
create_the_turtle = Turtle()

# set the turtle shape, color
create_the_turtle.shape('turtle')
create_the_turtle.color('blue')

item = 0
draw a square
while item < 4:
    create_the_turtle.fd(100) # forward 100 point
    create_the_turtle.right(90.0) # turn right 90 degrees
    item += 1

# draw a dashed line
item = 0
while item < 15:
    if item % 2 == 0:
        create_the_turtle.pendown()
        create_the_turtle.fd(10) # forward 100 point
    else:
        create_the_turtle.penup()
        create_the_turtle.fd(10)  # forward 100 point
    item += 1


# create a screen object
create_screen = Screen()

# prevent window close automatically, it will close if we click on it
create_screen.exitonclick()

```

### 畫三邊形到十邊形

``` python 
from turtle import Turtle, Screen

# create a turtle object
create_the_turtle = Turtle()

# set the turtle shape, color
create_the_turtle.shape('turtle')
create_the_turtle.color('blue')

item = 3
while item < 11:
   for _ in range(item):
       create_the_turtle.fd(100)
       create_the_turtle.right(360/item)
   item += 1

# create a screen object
create_screen = Screen()

# prevent window close automatically, it will close if we click on it
create_screen.exitonclick()

```

### Random Walk

``` python

from turtle import Turtle, Screen
import random
# create a turtle object
create_the_turtle = Turtle()

colours = ["CornflowerBlue", "DarkOrchid", "IndianRed", "DeepSkyBlue", "LightSeaGreen", "wheat", "SlateGray", "SeaGreen"]
# direction list
directions = [0, 90, 180, 270]
# 畫線寬度
create_the_turtle.width(10)
# 畫線速度
create_the_turtle.speed(10)

for _ in range(200):
    create_the_turtle.color(random.choice(colours))
    create_the_turtle.fd(30)
    create_the_turtle.setheading(random.choice(direction

```

### Make a Spirograph

``` python
import turtle as t
import random

t.colormode(255)

# create a turtle object
create_the_turtle = t.Turtle()

# make a spirogragp
def draw_spirograph(gap):
    for _ in range(int(360/gap)):
        create_the_turtle.color(random_color())
        # draw a circle with 100 radius
        create_the_turtle.circle(100)
        # get current heading
        current_heading = create_the_turtle.heading()
        # set turtle's heading
        create_the_turtle.setheading(current_heading + gap)
draw_spirograph(3.6)
# create a screen object
create_screen = Screen()

# prevent window close automatically, it will close if we click on it
create_screen.exitonclick()
```

### hirst spot painting - 

How to draw Extract RGB Value from Images : Use `colorgram` package

``` python 
import colorgram
import turtle as turtle_module
import random
# get color from a image
rgb_colors = []
colors = colorgram.extract('hirst_spot_image.jpg', 160)
for color in colors:
    r = color.rgb.r
    g = color.rgb.r
    b = color.rgb.b
    new_color = (r, g, b)
    rgb_colors.append(new_color)

# set turtle object attribute
turtle_module.colormode(255)
tim = turtle_module.Turtle()
tim.speed("fastest")
tim.penup()
tim.hideturtle()

tim.setheading(225)
tim.forward(300)
tim.setheading(0)
number_of_dots = 100

for dot_count in range(1, number_of_dots + 1):
    tim.dot(20, random.choice(color_list))
    tim.forward(50)
    if dot_count % 10 == 0:
        tim.setheading(90)
        tim.forward(50)
        tim.setheading(180)
        tim.forward(500)
        tim.setheading(0)
# create a screen object
create_screen = turtle_module.Screen()

# prevent window close automatically, it will close if we click on it
create_screen.exitonclick()
```