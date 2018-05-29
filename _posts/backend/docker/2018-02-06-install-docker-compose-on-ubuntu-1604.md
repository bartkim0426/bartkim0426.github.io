---
layout: post
title:  "ubuntu 16.04에 docker-compose 설치하기"
categories: docker
tags: django docker
---

다음 명령어로 간단하게 ubuntu 16.04에 docker-compose를 설치할 수 있다.


```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

설치 되었는지 버전 확인
```bash
docker-compose --version
```
