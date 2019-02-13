---
layout: post 
title:  "Ubuntu 18.04 - python (django) server setup"
categories: server
---


## Python setup

우선 apt-get update, upgrade를 비롯해 `pip3`을 설치하는 등 기본적인 절차 이후 파이썬, pip 버전을 확인해준다.

```
sudo apt-get update
sudo apt-get -y upgrade

# check python version
python3 --version

# install pip
sudo apt-get install -y python3-pip
pip3 --version

# Install more packages and dev tools for python
sudo apt-get install build-essential libssl-dev libffi-dev python-dev
```

## Virtual env

pyenv, virtualenv 등 원하는 env 설치 가능. 단일 서버로 사용할 것이기 때문에 그냥 virtualenv를 설치하였다.

```
# globally install
sudo -H pip3 install virtualenv

# create virtualenv
virtualenv project_env

source project_env/bin/activate
```

다음은 `venv`를 활용한 설치법.
```
sudo apt-get install -y python3-venv

mkdir environments
cd environments

python3 -m venv project_env
ls project_env

bin  include  lib  lib64  pyvenv.cfg  share

source project_env/bin/activate
```


## Uwsgi

한 서버에 하나의 서비스만 띄울 계획이므로 uwsgi 를 global하게 설치.

```
deactivate

sudo -H pip3 install uwsgi
uwsgi --version
```

테스트 프로젝트를 만들어서 확인

```
cd ~

mkdir testproject
cd testproject
virtualenv venv
source venv/bin/activate

(venv) pip install django
(venv) django-admin startproject testproject
(venv) cd testproject
(venv) python manage.py migrate
(venv) python manage.py runserver 0.0.0.0:8000
# 만약 안되면 ALLOWED_HOST = ['*'] 수정해준다.
```


```
# testproject로 작동하는지 확인
sudo uwsgi --http 0.0.0.0:8000 --home /home/username/testproject/test --chdir /home/username/testproject/testproject --wsgi-file /home/username/testproject/testproject/testproject/wsgi.py
```


ini 파일을 만들어준다. 

```
cd ~

mkdir uwsgi
cd uwsgi

mkdir sites
cd sites

vim myproject.ini
```

파일에 원하는 uwsgi ini 파일을 작성. 일단은 http를 활용해서 작동하는지 확인.

```
[uwsgi]
home = /home/username/testproject/test
chdir = /home/username/testproject/testproject
wsgi-file = /home/username/testproject/testproject/testproject/wsgi.py

http = 0.0.0.0:8000
```

이후 작동이 확인되면 socket으로 변경해준다.

```
[uwsgi]
home = /home/username/project_env
chdir = /home/username/project_name/project_dir
wsgi-file = /home/username/projecdt_name/project_dir/path_to/wsgi.py

for-readline = /home/path_to/uwsgi-env
  env = %(_)
endfor =
socket = /home/path_to/test.sock
vacuum = true
chmod-socket = 666

logto = /tmp/test.log

```

> uwsgi-env 파일은 '' 없이 작성해준다
예시
```
DJANGO_SETTINGS_MODULE=config.settings.production

DJANGO_SECRET_KEY=my_secret_key
POSTGRES_DB_NAME=test
POSTGRES_USER=test
POSTGRES_PASSWORD=test
DJANGO_ALLOWED_HOSTS=['*']

```

그리고 systemd를 활용할 service 파일을 작성해준다.

```
cd /etc/systemd/system/
sudo vim uwsgi.service
```

다음과 같이 작성해준다.

```
[Unit]
Description=uWSGI Emperor service
[Service]
# ExecStartPre=/bin/bash -c ‘mkdir -p /run/uwsgi; chown username:www-data /run/uwsgi’
ExecStart=/usr/local/bin/uwsgi --emperor /home/username/uwsgi/sites
Restart=always
KillSignal=SIGQUIT
Type=notify
NotifyAccess=all
[Install]
WantedBy=multi-user.target
```

## Nginx


```
sudo apt-get install nginx
sudo nginx -v

cd /etc/nginx/sites-available
vim project_name
```

```
server {
    listen 80;
    server_name {server};
    client_max_body_size 75M;
    include /etc/nginx/mime.types;

    location / {
        include uwsgi_params;
        uwsgi_pass unix:/home/path_to/crowdbase.sock;
    }

    location /media{
        alias /home/path_to/mediafiles;
    }

    location /static{
        alias /home/path_to/staticfiles;
    }
}
```

ln -s 명령어를 통해서 심볼릭 링크 설정

```
sudo ln -s /etc/nginx/sites-available/project_name /etc/nginx/sites-enabled

# syntax chck
sudo nginx -t

sudo systemctl enable nginx
sudo systemctl enable uwsgi
```

접속이 잘 되는걸 확인할 수 있다



