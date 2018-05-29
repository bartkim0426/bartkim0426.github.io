---
layout: post
title:  "django imagefield: make thumbnail image with dpi"
categories: django
tags: django pil
---

This is example for save thumbnail image from original image with dpi setting.

I use PIL library and `SimpleUploadedFile` for saving thumbnail image to `image_th` column.

```python
import os
import sys
import urllib2 as urllib
from PIL import Image
from cStringIO import StringIO
from django.core.files.uploadedfile import SimpleUploadedFile

# can use anyway
# this code is example for django model method
# use thumb_size for longer side (width or height)
def save_thumb(self, thumb_size):

try:
	# you can use your image column instead of image
	img_file = urllib.urlopen(self.image.url)
	im = StringIO(img_file.read())
	resized_image = Image.open(im)
	# check dpi
	try:
		dpi = resized_image.info['dpi']
	# if not have dpi, use default dpi
	except AttributeError:
		dpi = (96, 96)

	# for changing size
	# check width, height
	width, height = resized_image.size 

	if width > height:
		th_width = thumb_size
		th_height = thumb_size * height / width 
	else:
		th_height = thumb_size
		th_width = thumb_size * width / height

	th_size = (th_width, th_height)

	resized_image.thumbnail(th_size, Image.ANTIALIAS)

	# temp_save resized image
	temp_handle = StringIO()
	# save with adding dpi 
	resized_image.save(temp_handle, format='jpeg', dpi=dpi)
	temp_handle.seek(0)

	# save with model title field
	# use anything you want
	th_name = self.title + "_thumb"

	suf = SimpleUploadedFile(th_name, temp_handle.read(), content_type="image/jpeg")
	self.image_th.save("%s.jpg" % suf.name, suf, save=True)

except:
	# for printing error
	e = sys.exc_info()
	print "Something wrong with " + self.title
	print(e)
	pass
```

If you have any question, leave comment or contact me.
