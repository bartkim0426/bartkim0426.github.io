---
layout: post
title:  "Django: disable password check (password2) in django-allauth"
categories: django
---

`Django-allauth` support password check function. If want to remove this function, just add to `settings.py`

```python
ACCOUNT_SIGNUP_PASSWORD_VERIFICATION = False
```


> You can check other configuration in [allauth docs](http://django-allauth.readthedocs.io/en/latest/configuration.html)
