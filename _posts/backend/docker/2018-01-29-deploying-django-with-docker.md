---
layout: post
title:  "도커로 장고 배포하기 - docker-compose"
categories: docker
tags: django docker
---


Docker Django
-------------

## Usage
```bash
$ git clone https://github.com/bartkim0426/django-docker-seul.git
# when delpoy
$ docker-compose up
# when development
$ docker-compose -f docker-compose-dev.yml up
```

Now you can access the application at <https://localhost> and the admin site
at <https://localhost/admin>.

## Settings
### Have to change `development.env` and `development_local.env`
- `NGINX_SERVER_NAME` : add server name
### `uwsgi` settings
- in `webapp/config/django-uwsgi.ini`
### `Nginx` settings: HAVE TO set up before build image
- In `services/webserver/config/nginx.tmpl`

## Processes (deploying)
### 1. Build and run `db`
- Using `postgres:9.6` image
- Create volumes at `psql-data` in project dir. (Using `development.env`)


### 2. Build and run `webapp`
- using `webapp/Dockerfile` to build. 
- Add `webapp/config/requirements.txt`, `webapp/config/start.sh/`, `webapp/starter` to projectand `pip install`
- Run `adduser` and `chown` commands
- Run `webapp/config/start.sh`: migrate => collectstatic => starting uWSGI


### 3. Build and run `webserver`
- Using `services/webserver/Dockerfile` to build
- Add `start.sh`, `nginx.tmpl`
- Run `start.sh` => Starting Nginx


## Following To-do list
- `chmod 755` to `media` directory in `webapp`, `webserver`
- `postgres` database volumes checking
- Not deploy at once

