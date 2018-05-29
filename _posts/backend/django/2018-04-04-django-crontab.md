---
layout: post
title:  "Django: use crontab easy in django - django-crontab"
categories: django
---

### Install

Insatll via pip

```bash
pip install django-crontab
```

And add it to `INSTALLED_APPS`

```python
INSTALLED_APPS = (
    'django_crontab',
    ...
)
```

### Make `cron.py`

Create `cron.py` in project directory (same directory as `settings.py`)

```python
def my_scedulaer():
    # do sth
    pass
```

And add `CRONJOBS` in `settings.py`

```python
CRONJOBS = [
    ('*/5 * * * *', 'myapp.cron.my_scheduled_job'),
    # also can add args, kwargs
    ('*/5 * * * *', 'myapp.cron.other_scheduled_job', ['arg1', 'arg2'], {'verbose': 0}),
]
```

### Commands

```bash
# showing current crontab jobs
python manage.py crontab show

# adding crontab
python manage.py crontab add

# remove all jobs
python manage.py crontab remove
```


If you want more information, please check [django-crontab github(here)](https://github.com/kraiz/django-crontab)
