---
layout: post
title:  "django: using queryset chainable model manager"
categories: django
---

In django, you can use your custom model manager. 

I usally add `display()` for filtering `is_display` and `created_for_day()` for filtering by `created_at()`.

It's really easy to add custom model manager.

### Adding custom model manager 
Add custom model manager in `models.py` (or anywhere you want), and add it to model.
```python
class DisplayManager(models.Manager):
    def get_queryset(self):
        return super(DisplayManager, self).get_queryset().filter(is_display=True)

class Post(models.Model):
    # you have to add this for using default objects() manager
    objects = models.Manager()
    display = DisplayManager()
    ...
    is_display = models.BooleanField(default=True)
    ...

```

After that, you can simply use `display` for Post model.

```python
>>> from post.models import Post
>>> Post.objects.count()
2
>>> Post.display.count()
1
```

### Add custom model manager as method chain
But if you add custom model manager by that way, it has few problems: I can't use both managers, and it's not pythonic way at all.

So you can add custom manager chainable. Just add `use_for_related_fields` to custom model manager and set default model managers to custom one.

```python
class DisplayManager(models.Manager):
    use_for_related_fields = True

    def get_queryset(self):
        return super(DisplayManager, self).get_queryset().filter(is_display=True)

class Post(models.Model):
    objects = DisplayManager()
    ...
    is_display = models.BooleanField(default=True)
    ...
```

Then you can use manager chainable.


```python
>>> from post.models import Post
>>> Post.objects.count()
2
>>> Post.objects.published().count()
1
```


It looks great!!

However, it has a small problem. I can't use this manager after queyset. What if you want to filter by display manger after get queryset? You can't use your manager after queryset.

```python
>>> from post.models import Post
>>> posts = Post.objects.all()
# I want 'posts.objects.display()' or sth
```

### Use QuerySet and add it by as_manager

Here's solution. You can use `models.query.QuerySet` instead of `models.Manager`. Then add it to model using `as_manager`

```python
class BaseQuerySet(models.query.QuerySet):
    def display(self):
        queryset = self.filter(is_display=True)
        return queryset

class Post(models.Model):
    objects = BaseQuerySet.as_manager()
    ...
    is_display = models.BooleanField(default=True)
    ...
```

Then magic happens! You can use your new custom qureyset anytime! 
```python
>>> from post.models import Post
>>> posts = Post.objects.all()
>> posts.display().count()
1
```

### Plus tip: inherit BaseQuerySet per models
If you have all managers per model, it's hard to managing your code. So you can inherit `BaseQuerySet` that you make, and make new custom queryset for other models.


```python
class PostQuerySet(BaseQuerySet):
    def deadline_for(self, day):
        today = datetime.date.today()
        td = today - datetime.timedelta(days=day)
        queryset = self.filter(deadline__gte=td).filter(is_display=True).order_by('-created_at')
        return queryset

class Post(models.Model):
    objects = PostQuerySet.as_manager()
    ...
    is_display = models.BooleanField(default=True)
    ...
```

Then you can use chainable manager whatever you want.

```python
>>> from post.models import Post
>>> posts = Post.objects.all()
>> posts.display().count()
1
>> post_within_deadline = Post.objects.display().deadline(15)
```
