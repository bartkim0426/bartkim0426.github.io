---
layout: post
title:  "Django: django-redis install and basic usage"
categories: django
---


### Install

```bash
pip install django-redis
```

### Configuration

Add `CACHES` in `settings.py`

```python
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
    }
}
```

### Configure session backend

If want to use session backend with `django-redis`, add `SESSION_ENGINE` and `SESSION_CACHE_ALIAS` to `settings.py`

```python
SESSION_ENGINE = "django.contrib.sessions.backends.cache"
SESSION_CACHE_ALIAS = "default"
```


> For more information, check [django-redis documentatino](https://niwinz.github.io/django-redis/latest/)
