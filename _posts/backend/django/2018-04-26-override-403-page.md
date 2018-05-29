---
layout: post
title:  "Django: overriding 403 page"
categories: django
---


In `settings.py`, can add `CSRF_FAILURE_VIEW='app_name.views.csrf_failure'`

Then add `csrf_failure` in your view.

```python
def csrf_failure(request, reason=""):
    ctx = {'message': 'custom message'}
    return render_to_response(custom_template, ctx)
```


> More details in [documentation](https://docs.djangoproject.com/en/dev/ref/settings/#csrf-failure-view)
