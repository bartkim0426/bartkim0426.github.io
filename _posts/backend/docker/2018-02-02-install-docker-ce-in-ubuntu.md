---
layout: post
title:  "ubuntu 16.04에 docker-ce (Community Edition) 설치하기"
categories: docker
tags: django docker
---


우분투에 도커를 설치하는 것은 어렵지 않지만, 맥에서 프로그램을 다운로드 받는 것보다는 이것저것 해 주어야 할 것이 많다. 

[공식 문서](https://docs.docker.com/install/linux/docker-ce/ubuntu/)에서 ubuntu에 Docker CE를 설치하는 방법을 볼 수 있다. 

조금 더 쉽고 간편하게 확인할 수 있기 위해 정리 해 둔다.


### Prepare
```bash
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable"
sudo apt-get update
```

### Following command
```bash
apt-cache search docker-ce
```

### Install docker
```bash
sudo apt-get install docker-ce
```
