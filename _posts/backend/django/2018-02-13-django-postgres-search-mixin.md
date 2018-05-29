---
layout: post
title:  "django: postgres full text search mixin"
categories: django
---

When using postgresql with django, you can use Full text search. It's really easy way to search text in many fields, at once such as `title`, `content`, and so on.

> You can see detail information in [official django docs - Full text search](https://docs.djangoproject.com/en/2.0/ref/contrib/postgres/search/)

Here's example using `SearchVector` to search in `body_text` and `blog__tagline` both.

```python
>>> from django.contrib.postgres.search import SearchVector
>>> Entry.objects.annotate(
...     search=SearchVector('body_text', 'blog__tagline'),
... ).filter(search='Cheese')
[<Entry: Cheese on Toast recipes>, <Entry: Pizza Recipes>]
```

In `ListView`, you can simply add searching function in `get_queryset()`

```python
class PostListView(ListView):
    model = Post
    context_object_name = "posts"
    template_name = "post/list.html"

    def get_queryset(self):
        queryset = super(FilterMixin, self).get_queryset()

        searching = self.request.GET.get("searching", "")

        if searching != "":
            search_kwgs = SearchVector('title', 'description')
            queryset = queryset.annotate(
                search=search_kwgs).filter(search=searching).distinct()
        else:
            pass
        return queryset
```

But if your app has many model (and also many `ListView`) for searching fields, it's little bit annoy to use this searching method. So I made my custom `BaseSearchMixin` to use easily.

```python
from django.contrib.postgres.search import SearchVector


class BaseSearchMixin(object):

    def get_queryset(self):
        queryset = super(BaseSearchMixin, self).get_queryset()
        if not self.search_fields or self.request.GET.get('searching', '') is None:
            return queryset

        return queryset.annotate(
            search=SearchVector(*self.search_fields)
        ).filter(
            search__icontains=self.request.GET.get('searching', '')
        )
```

You can inherit this `BaseSearchMixin` with `search_fields`.

```python
class PostListView(BaseSearchMixin, ListView):
    model = Post
    context_object_name = "posts"
    template_name = "post/list.html"
    search_fields = ('title', 'description')
```

Done, looks great! 
