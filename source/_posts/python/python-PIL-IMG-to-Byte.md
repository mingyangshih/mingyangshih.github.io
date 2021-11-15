---
title: Python change data to bytes without saving to disk
date: 2021-11-10 13:56:11
description: Python change data to bytes without saving to disk
categories: python
tags:
  - python
---


``` python 
import io
from PIL import Image

im = Image.open('test.jpg')
im_resize = im.resize((500, 500))
buf = io.BytesIO()
im_resize.save(buf, format='JPEG')
byte_im = buf.getvalue()

```
* [參考資料](https://jdhao.github.io/2019/07/06/python_opencv_pil_image_to_bytes/)

### JSON data to binary type
``` python

# change json to binary string
data = json.dumps(route_coords).encode("ascii")

```