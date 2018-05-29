---
layout: post 
title:  "Using Environment variable in uswgi.ini"
categories: server
---


Just add `env = ENV`

```
[uwsgi]
master = true
processes = 2
threads = 2
socket = /tmp/uwsgi.sock
chmod-socket = 666
chdir = /var/www/service/my_site/
module = my_site.wsgi
home = /home/ubuntu/my_site
vacuum = true
env = DJANGO_SETTINGS_MODULE=settings_dev

uid = www-data
gid = www-data
# daemonize2 = logs/uwsgi.log
```
