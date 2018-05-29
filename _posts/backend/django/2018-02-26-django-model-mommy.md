---
layout: post
title:  "django model test - model_mommy"
categories: django
---

I felt wierd when make testing in django model, so I usally skipped model testing. But I found great library to help testing model, `model_mommy`


### Intall
install with pip
```bash
pip install model_mommy
```

### Usage
Basic usage is very simple. Just use with `mommy.make(model)`
```python
from django.test import TestCase

from model_mommy import mommy

from .models import Post

class PostTestModel(TestCase):

    def setUp(self):
        self.kid = mommy.make(Kid)
    ...
```

Also can make relational model object.

```python
from django.test import TestCase

from model_mommy import mommy


class CommentTestModel(TestCase):

    def setUp(self):
        self.comment = mommy.make('post.Comment')
        # if using m2m
        self.comment = mommy.make('post.Comment', make_m2m=True)
```
