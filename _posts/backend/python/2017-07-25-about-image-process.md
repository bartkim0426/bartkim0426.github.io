---
layout: post
title:  "python image process with PIL"
categories: python
---

## PIL

### Fast steps to make new img from average rgb
```python
from PIL import image
from colorthief import ColorThief

im = Image.open('path/to/image.png')
# get img pixels
list_of_img_pixels = list(im.getdata())
# get average rgb data
average_rgb = tuple([sum(x)/len(x) for x in zip(*list_of_img_pixels)])

# make new img from average_rgb
# you can adjust mode, size

color_thief = ColorThief('path/to/image.png')
dominant_color = color_thief.get_color(quality=1)

new_img_with_color = Image.new(im.mode, im.size, color=average_rgb)
new_img_using_colorthief = Image.new(im.mode, im.size, color=dominant_color)
```

### 기본 Python Image Libraly 사용법들


```python
from PIL import Image

im = Image.open('path/to/image.png')

# basic property
im.mode
im.size

# show images
im.show()

# get img pixel data
im.getdata()

# making new images
new_im = Image.new()

# how to create crop img
box = (100, 100, 400, 400)
im_crop = im.crop(box)
```

### Making new image with rgb pixel hexs

```python
list_of_img_pixels = list(im.getdata())
img_with_data = Image.new(im.mode, im.size)

# if you want exactly same img, use putdata
img_with_data.putdata(list_of_img_pixels)

# or you can make img with rgb color data
img_with_color = Image.new(im.mode, im.size, color=(0,0,0,0))
```

### Get the sum of RGB hex of img
```python
from pil import Image

im = Image.open('path/to/img.png')
list_of_img_pixels = list(im.getdata())

average_rgb = [sum(x)/len(x) for x in zip(*list_of_img_pixels)]
```


## [Color Thief](https://github.com/fengsp/color-thief-py)
- get dominant color from img to RGB hex tuple
- install by `pip install colorthief`
- don't know about how it generate dominant color: it's little bit different from average rgb from `im.getdata()` tuples

### Usage
```python
from colorthief import ColorThief

color_thief = ColorThief('path/to/image.png')
# get dominant color
dominant_color = color_thief.get_color(quality=1)
# make a color palette
palette = color_thief.get_palette(color_count=6)
```
