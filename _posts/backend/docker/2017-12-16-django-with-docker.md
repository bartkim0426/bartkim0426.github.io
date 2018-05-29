---
layout: post 
title:  "Docker - django 2.0 localhost 띄워보기"
categories: docker
tags: docker django
---

요즘 docker의 매력에 푹 빠졌다. 아직은 container, image 등 기초적인 것들을 공부하고 있지만 기본적인 dockerfile로 django runserver을 docker에서 구현해보았다. 


추후 실제 배포 단계에서도 nginx, uwsgi나 gunicorn을 활용해서 docker로 배포한다면 정말 좋을 것 같다. (많이 늦은 감이 있지만.. 늦은게 아예 안 하는 것보다는 낫겠지?)


### django project 구성
이건 본인의 입맛에 맞게 project를 구성하면 된다. `django-admin startproject`를 해도 좋고, 자신만의 project-base를 만들어도 좋다. 어쨌건 [django 2.0 공식문서](https://docs.djangoproject.com/en/2.0/releases/2.0/)를 토대로 local 환경에서 runserver가 구동되게만 세팅해 놓는다.


### Dockerfile 만들기
django project_base directory에 `Dockerfile`을 만들어준다.

이후 자신이 쓰는 버전에 맞는 python image를 기본으로 dockerfile을 작성한다. 

> 이전에는 [django 공식 도커 이미지](https://hub.docker.com/_/django/)가 있었는데, 2016년 12월 31일을 기준으로 deprecated 되어서 python 이미지를 base로 dockerfile을 작성하면 된다.

[python 공식 도커 이미지(docker hub)](https://hub.docker.com/_/python/)에 들어가서 자신이 사용하는 버전의 파이썬 image를 확인한다. 나는 3.6.0 버전을 사용중이기 때문에 그냥 `latest`를 사용하였다.

> 17.12.16 기준 `3.6.3`이 latest로 되어있다.

```
# latest기 때문에 그냥 python만 적어줌
# 버전을 적으려면 
# FROM python:2.7 이런식으로 적어준다.
FROM python

# 필요한 것들을 install
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        postgresql-client \
    && rm -rf /var/lib/apt/lists/*

# requirements.txt를 복사해서 pip install
# 본인의 requirements 에 맞게 수정해준다.
# COPY . . 를 통해 모든 코드 복사
WORKDIR /usr/src/app
COPY requirements.txt ./
RUN pip install -r requirements.txt
COPY . .

# 원하는 포트를 열어준다. runserver가 8000번을 default로 쓰기때문에 8000번을 열어줌
EXPOSE 8000

# manage.py가 있는 directory로 이동한 후에 runesrver 실행
# manage.py가 dockerfile과 같은 dir에 있으면 따로 WORKDIR를 지정할 필요는 없다.
WORKDIR /usr/src/app/django_20
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

### 만들어진 dockerfile로 이미지 build
```
# -t 는tag 지정. image repository name을 지정해준다.
$docker image build -t django-test

# 만들어진 image 확인
$docker image ls

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
django-test         latest              e5908171d989        16 minutes ago      729MB
```

### Run container
```
# 기본 80번 포트를 위에서 EXPOSE한 8000포트에 물려준다.
# --rm 명령어는 container가 stop되면 자동으로 삭제하기 위해서. 쓰지 않아도 무방함.
$docker container run -p 80:8000 --rm django-test
```

정상적으로 runserver가 되면 웹브라우저에서 `localhost`로 장고 2.0 default page (로켓!)가 뜨는 것을 확인할 수 있다! 

![django-2.0-starting-page](https://i.imgur.com/Uta5BHI.png){:class="img-responsive"}


해피해킹!!
