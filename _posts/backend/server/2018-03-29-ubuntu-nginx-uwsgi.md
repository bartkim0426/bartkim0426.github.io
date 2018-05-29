---
layout: post 
title:  "ubuntu 14.04 uwsgi, nginx with django"
categories: server
---


## Install virtualenv

### Install pip, virtualenv, virtualenvwrapper

```bash
sudo apt-get install -y python-pip python-virtualenv virtualenvwrapper  
sudo apt-get install python-dev
```

### Make virtualenv dirs
```bash
mkdir ~/virtualenvs
```

### Add configure to `.bashrc`

```bash
export WORKON_HOME=~/virtualenvs  
source /usr/share/virtualenvwrapper/virtualenvwrapper.sh  
export PIP_VIRTUALENV_BASE=~/virtualenvs  
```

### Create virtualenv
```bash
mkvirtualenv mysite
workon mysite
```


## Install nginx

```bash
sudo apt-get install nginx
```

Before add custom nginx file, just comment out `default` nginx (`/etc/nginx/site-avaliable/default`)

### add nginx conf
In `/etc/nginx/site-avaliable` , add your site nginx conf. (ex. `mysite`)

```nginx
server {
        listen 80;
        server_name your_server_name;
        access_log /var/log/nginx/mysite_access.log;
        error_log /var/log/nginx/mysite_error.log;

        location / {
                uwsgi_pass 0.0.0.0:3031;
                include uwsgi_params;
        }

        location /media/ {
                alias /home/username/mysite/media/;
        }

        location /static/ {
                alias /home/username/mystie/static/;
        }
}
```

### `ls` tn `/etc/nginx/site-enable`
```bash
sudo ln -s /etc/nginx/site-avaliable/mysite /etc/nginx/site-enable
```


## Uwsgi settings

```bash
pip install uwsgi
```

If needed, install `uwsgi-plugin-python`

```bash
sudo apt-get install uwsgi-plugin-python
```
> need more information, check [SO answer](https://stackoverflow.com/questions/10748108/nginx-uwsgi-unavailable-modifier-requested-0)


Make project uwsgi file in `/etc/uwsgi/apps-avaliable`

example of `mysite.ini`
```bash
[uwsgi]
vhost = true
plugins = python
socket = 0.0.0.0:3031
master = true
enable-threads = true
processes = 4
wsgi-file = /home/username/project/path_to/wsgi.py
virtualenv = /home/username/virtualenvs/project
chdir = /home/username/project
touch-reload = /home/username/project/reload
```

### `ls` to `/etc/uwsgi/apps-enable`
```bash
sudo ln -s /etc/uwsgi/apps-avaliable/mysite.ini /etc/uwsgi/apps-enable
```


## Starting nginx and uwsgi

```bash
sudo service nginx start
sudo service uwsgi start
```
