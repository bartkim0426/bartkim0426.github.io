---
layout: post
title:  "Django: add extra context to admin changelist view"
categories: django
---



```python
class MyModelAdmin(admin.ModelAdmin):
    ...
    def changelist_view(self, request, extra_context=None):
        extra_context = extra_context or {}
        extra_context['my_context'] = 'extra test context'
        return super(MyModelAdmin, self).changelist_view(request, extra_context=extra_context)
```


> more detail check [django docs](https://docs.djangoproject.com/en/dev/ref/contrib/admin/#django.contrib.admin.ModelAdmin.changelist_view)
