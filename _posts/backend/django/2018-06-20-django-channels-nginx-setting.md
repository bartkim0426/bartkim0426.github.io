---
layout: post
title:  "Django: channels 2.0 with nginx + daphne"
categories: django
---


Recently I made chat function with django channels 2.0

It's bit easy to create function in local, but I encountered obstacle while deploy it to production server.


I have to use `asgi` for deploy socket. For that, I use `daphne` instead of `gunicorn` or `uwsgi`.



### Install redis in ubuntu

First, just redis. It's not a hard work.

```bash
sudo apt-get install redis-server

# then start server
redis-server start

# can access redis-cli
redis-cli
```

### Install channel_redis

Have to install dependencies just like local's `requirements.txt`

```bash
pip install -U channels_redis
```

### Add `asgi.py`

Have to make `asgi.py` just beside `wsgi.py`

```python
"""
ASGI entrypoint. Configures Django and then runs the application
defined in the ASGI_APPLICATION setting.
"""

import os
import django
from channels.routing import get_default_application

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "myproject.settings")
django.setup()
application = get_default_application()
```


### use daphne

```bash
daphne myproject.asgi:application
```

> for more information about daphne, check [here](https://github.com/django/daphne)


### nginx settings

```nginx
server {
    listen 80;
    server_name my_site_name;
    client_max_body_size 30M;
    # plus your own settings
    ...

    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_pass http://127.0.0.1:8000;

        location /ws {
            proxy_pass http://127.0.0.1:8000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }
    }
    # your settings include /static or /media
    ...
}
```
