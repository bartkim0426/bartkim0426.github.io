---
layout: post
title:  "Docker hub에 만들어진 image 올리기"
categories: docker
tags: docker hub
---

docker hub에 이미지 올리는 방법 정리

### 1. image에 tag 부여하기
```
# 잘 모르겠을 경우 docker image tag --help로 help command 보기
# $docker image tag image_name DOCKER_HUB_USERNAME/image_name:version

# 예시 - django_test 이미지에 태그달기
# latest일 경우에는 생략 가능
$docker image tag django_test bartkim0426/django_test:latest
```

### 2. docker hub login
```
$docker login
# 이후 나온 command line에 username, password 입력
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username (bartkim0426):
Password:
Login Succeeded
```

### 3. push image to docker hub

```
# $docker image push DOCKER_HUB_USERNAME/image_name
$docker image push bartkim0426/django-test
```
