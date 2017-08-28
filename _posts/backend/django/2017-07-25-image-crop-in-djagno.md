---
layout: post
title:  "image crop in django"
categories: django
---

## image cropping in django

for cropping thumbnail image from image of artwork, I use some libraries.
- django-imagekit: 
- django-image-cropping
- django-versatileimagefield
- easy-thumbnail 등...


### [django-image-cropping](https://github.com/jonasundderwolf/django-image-cropping)

```python
from django.db import models
from image_cropping import ImageRatioField

class MyModel(models.Model):
    image = models.ImageField(blank=True, upload_to='uploaded_images')
    # size is "width x height"
    cropping = ImageRatioField('image', '430x360')
```

### Python Image Library의 ImageOps를 사용하여 crop하기

- [PIL.ImageOps 문서](http://pillow.readthedocs.io/en/3.1.x/reference/ImageOps.html)

```python
from PIL import Image, ImageOps

img = Image.open('path/to/image.png')
thumb_size = (250, 230)
thumb_img = ImageOps.fit(img, thumb_size, Image.ANTIALIAS)
```

