---
title: Change the Python3 default Version in Ubuntu
date: 2021-07-06
description: Linux note
categories: Linux
tags: 
  - python
  - Linux
  - Ubuntu
---

By default python on mostly ubuntu, there is python 2. We need to use python3 to run the python files with the latest version.

Steps to Set Python3 as Default On ubuntu?
* Check python version on terminal - python --version
* Get root user privileges. On terminal type - sudo su
* Write down the root user password.
* Execute this command to switch to python 3.X. update-alternatives --install /usr/bin/python python /usr/* bin/python3 1
* Check python version - python --version
* All Done!
