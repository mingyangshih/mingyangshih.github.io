---
title: 100 Days of Code - Dictionaries, Nesting (Day9)
date: 2021-09-28 22:53:28
description: Dictionaries, Nesting
categories: python
tags:
  - python
---

### Dictionary

``` python

# definition a dictionary
{key: value}

dictionary = {
  "key1": "value1",
  "key2": "value2",
  "key3": "value3"
}

# retrieve data from dictionary
dictionary["key1"] # value1
dictionary["key2"] # value2

# add data to dictionary
dictionary["key4"] = "value4"

# wipe an existing dictionary
dictionary = {}

# Loop through a dicrionary
for key in dictionary:
  print(key) # will print key

# Nesting Lists and Dictionaries
{
  key: [List],
  key1: {Dict}
}
```