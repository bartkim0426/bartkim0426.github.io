---
layout: post
title:  "PIL (pillow)를 사용해서 dpi, size 변경하기"
categories: python
tag: python PIL pillow
---

테스트 환경: python 3.6.1, Pillow 4.3.0, jupyter notebook

## PIL 사용법

```python
from PIL import Image

path = '/Volumes/SOMAKIT/test.jpeg'
img = Image.open(path)
# check dpi
img.info['dpi']

# resizing img
# Image.ANTIALIAS는 좀 더 좋은 품질의 이미지를 저장(?)
size = (250, 250)
img = img.resize(size, Image.ANTIALIAS)

# dpi를 정의한 후 저장
# 새로 저장하려면 new_path 지정, 아니면 path 사용
new_path = '/Volumes/SOMAKIT/test.jpeg'
dpi = (80, 80)
img.save(path, dpi=dpi)
```
