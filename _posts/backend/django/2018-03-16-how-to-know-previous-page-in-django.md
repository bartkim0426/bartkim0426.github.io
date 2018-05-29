---
layout: post
title:  "django: how to know previous page in django"
categories: django
---


Sometimes I need previous page for some reasons.

- Redirect to previous page after delete object in detail page
- Redirect to previous page when login/loggout
...


You can know previous page with `request`.

Django request has many information with `META` tag. It has many useful data such as browser information, site domain, and so on.


In [here](https://docs.djangoproject.com/en/2.0/ref/request-response/#django.http.HttpRequest.META), you can check example of  `request.META` headers.

This `META` also has previous page information - `HTTP_REFERER`. If exist, it has it's referring page.

So you can use like this 

### In views
```python
prev_page = request.META.get('HTTP_REFERER')
```

### In templates
{% raw %}
```python
{{ request.META.HTTP_REFERER }}
```
{% endraw %}
