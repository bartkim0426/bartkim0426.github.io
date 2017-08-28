---
layout: post
title:  "Nginx + Gunicorn in Django"
categories: django nginx gunicorn
---

# Nginx + Gunicorn in Django

My project name is `myproject` and you should change it to your project name.

## Setting django
it's not django post, so you have to check other post. I recommend [django official tutorial](https://www.djangoproject.com/start/)
```bash
pip install django
mkdir repo run
sudo chown username:www-data run
cd repo
django-admin startproject myproject .
./manage.py makemigrations
./manage.py migrate
./manage.py runserver 0.0.0.0:8000
```

## Gunicorn setting

### Installing gunicorn
```bash
pip install gunicorn
```

### Testing gunicorn
```bash
gunicorn --bind 0.0.0.0:8000 myproject.wsgi:application
```

### Making service script
edit `/etc/init/gunicorn.conf` file.
you should change `username` and `myprjoect` to your username/project.    
Also you have to change `myprojectenv` to your virtualenv name.
```bash
description "Gunicorn application server handling myproject"

start on runlevel [2345]
stop on runlevel [!2345]

respawn
setuid username
setgid www-data
chdir /home/username/myproject

exec virtualenv/myprojectenv/bin/gunicorn --workers 3 --bind unix:/home/username/myproject/myproject.sock myproject.wsgi:application
```


### Starting gunicorn
```bash
sudo service gunicorn start
# few commands usage
sudo service gunicorn stop
sudo service gunicorn reload
sudo service gunicorn restart
```


## Config Nginx to Proxyt pass to gunicorn

### edit site-available
```bash
sudo vim /etc/nginx/sites-available/myproject
```

and add those line.   
you shold change `username` and `myproject` to fit 

```bash
server {
    listen 80;
	server_name server_domain_or_IP;

	location = /favicon.ico { access_log off; log_not_found off;  }

	location /static/ {
		root /home/user/myproject;
				
	}
	location / {
		include proxy_params;
		proxy_pass http://unix:/home/username/myproject/myproject.sock;
	}
}
```
### make linking to site-enable
`sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled`


### Starting Nginx
```bash
sudo service nginx restart
```
