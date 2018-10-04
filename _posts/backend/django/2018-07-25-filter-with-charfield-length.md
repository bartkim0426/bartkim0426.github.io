---
layout: post
title:  "Django: Filter with CharField length"
categories: django
---


## Use `Length`


```python
from django.db.models.functions import Length

qs = queryset.annotate(text_len=Length('text_field')).filter(text_len__gt=10)
```

> Only can use Django >= 1.8


## Use `extra`

```python
qs = queryset.extra(where=["CHAR_LENGTH(text_field) > 10"])
```



## Use `regex` inside filter

```python
qs = queryset.filter(text_field__regex=r'.{10}.*')
```


## Register `Length` to `CharField` lookup

```python
from django.db.models import CharField
from django.db.models.functions import Length

CharField.register_lookup(Length, 'length')

qs = queryset.filter(text_field__length__gt=10)
```

> Only can use Django >= 1.9
