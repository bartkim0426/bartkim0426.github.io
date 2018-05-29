---
layout: post
title:  "Django: logout when get for django-allauth"
categories: django
---


Recently I use `django-allauth`. (github [here](https://github.com/pennersr/django-allauth)). It's useful for social login, and upgrading functions for default login.


When using `logout`, allauth basically supply logout confirm page.

If you want to remove this page, and just logout when get (`/accounts/logout`), add this to `settings.py`


```python
ACCOUNT_LOGOUT_ON_GET = True
```
