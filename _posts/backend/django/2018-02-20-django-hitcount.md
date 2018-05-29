---
layout: post
title:  "django hitcount: counting hit (views) in django"
categories: django
---

It's not that difficult to counting every detail page in your website. 

But I found very easy and great library for that - `[django-hitcount](https://github.com/thornomad/django-hitcount/blob/master/example_project/blog/models.py)`


## Installing
using pip
```bash
pip install django-hitcount
```

and add it to `INSTALLED_APPS`
```python
# settings.py
INSTALLED_APPS = (
    ...
    'hitcount'
)
```

> Also you can use several settings - active days, ip limit, exclude user group and so on. check [here](http://django-hitcount.readthedocs.io/en/latest/settings.html) for more information.

## Usage

You should inherit hitcount for model, and detailview.

### Model
Just inherit `HitCountMixin`. And that's all!

```python
from hitcount.models import HitCount, HitCountMixin


@python_2_unicode_compatible
class Post(models.Model, HitCountMixin):
    title = models.CharField(max_length=200)
    content = models.TextField()
    ...

    def __str__(self):
        return "Post title: %s" % self.title
```

### View

You just inherit `HitCountDetailView` and add `count_hit = True`.
```python
from hitcount.views import HitCountDetailView

from community.models import Post


class PostDetailView(HitCountDetailView):
    model = Post
    count_hit = True
    ...
```


### Template

{% raw %}
```python
{# the primary key for the hitcount object #}
{{ hitcount.pk }}

{# the total hits for the object #}
{{ hitcount.total_hits }}
```
{% endraw %}
