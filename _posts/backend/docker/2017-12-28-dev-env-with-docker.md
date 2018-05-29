---
layout: post
title:  "도커로 장고 개발서버 구축 (Django dev server with docker)"
categories: docker
tags: django docker
---


I'm trying to use docker when deploying for this django project. Docker is useful for deploying server, but I try to use docker for dev environment server for some reason: 

- get familiar with docker
- easy to manage dev environment for all team members
- hard to manage many python environment in local (althoug use pyenv or virtualenv, it's always confusing)

So I made dev server with docker


## Precedence knowledge
If you are first time in docker, this example is little bit difficult to follow. you might have
- basic knowledge about docker image and container
- how to run docker image
- basic useage about github (clone)

## Try with docker image and container

### Get example source code
You can get example source code from my github [this page](https://github.com/bartkim0426/django-dev-with-docker). 

```bash
$ git clone https://github.com/bartkim0426/django-dev-with-docker .
```

### Build image from local
Create image from example code source. When using `docker build <path>`, it will automatically build image from `Dockerfile`

```bash
$docker build -t django-dev .
```
> `-t` option is for tag. you can build image with your own tag.

### Run postgres container 
you shold run postgres server for dev db. it's easy with postgres official image from `docker hub`
```bash
docker run -it --rm \
	--name postgres \
	-e POSTGRES_DB=djangodev \
	-e POSTGRES_USER=exampleuser \
	-e POSTGRES_PASSWORD=examplepassword \
	--volume=$(pwd)/docker/data:/var/lib/postgresql/data \
	postgres:9.6.3
```

- with `--rm` option, docker automatically remove container when container stop
- with `-e` option, you can give environ variables
- with `--volume`, you can keep data beside docker container: postgres is db so you should protect data from auto-deleting

### Run django container
```bash
docker run -it --rm \
	-p 8000:8000 \
	--link postgres \
	-e DJANGO_DB_HOST=postgres \
	-e DJANGO_DEBUG=True \
	--volume=$(pwd):/app/ \
	django-dev \
	./manage.py runserver 0.0.0.0:8000
```
- with `--link` option, you can connect `django-dev` container to `postgres` container
- with `--volume`, you can keep your source code seperately 		

After all this, you can access dev server with `localhost:8000`!



## Using `docker-compose`
With about approach, there's many limit
- it's really difficult to type all docker run options
- have to start two container with order: postgres => django

When using docker-compose, you can create and start many container at once, with `docker-compose.yml`

```docker
version: '2.1'

volumes:
  django_sample_db_dev: {}
  django_sample_packages: {}

services:
  db:
    image: postgres:9.6.1
    volumes:
      - django_sample_db_dev:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=sampledb
      - POSTGRES_USER=sampleuser
      - POSTGRES_PASSWORD=samplesecret
      - POSTGRES_INITDB_ARGS=--encoding=UTF-8
    healthcheck:
      test: "pg_isready -h localhost -p 5432 -U postgres"
      interval: 3s
      timeout: 1s
      retries: 10

  django:
    build:
      context: .
      dockerfile: ./compose/django/Dockerfile-dev
    environment:
      - DJANGO_DEBUG=True
      - DJANGO_DB_HOST=postgres
      - DJANGO_DB_PORT=5432
      - DJANGO_DB_NAME=sampledb
      - DJANGO_DB_USERNAME=sampleuser
      - DJANGO_DB_PASSWORD=samplesecret
      - DJANGO_SECRET_KEY=dev_secret_key
    ports:
      - "8000:8000"
    depends_on:
      db:
        condition: service_healthy
    links:
      - db:postgres
    command: /start-dev.sh
    volumes:
      - ./manage.py:/app/manage.py
      - ./requirements.txt:/app/requirements.txt
      - ./djangosample:/app/djangosample
      - django_sample_packages:/usr/local/lib/python3.6/site-packages/
```



