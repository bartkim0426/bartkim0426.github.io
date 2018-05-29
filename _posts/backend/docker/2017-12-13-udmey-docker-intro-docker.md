---
layout: post 
title:  "Docker intro-version, install"
categories: docker
tags: docker
---

## Docker Version
- Docker CE (Community Edition) & EE(Enterprise Edition) - Stable & Edge
- [docker store](https://store.docker.com/search?type=edition&offering=community)에서 다양한 docker version을 볼 수 있다.
- 나는 무료이고 오픈 소스인 docker CE의 mac os 버전의 Edge를 다운로드 하였다. 

> Yosemite 이전의 os를 사용한다면 (Leopard, Lion 등...) `Docker Toolbox`를 사용해서 설치해주어야 한다.
> Linux VM을 사용해서 Docker을 설치해도 무방
> `homebrew`로 설치해도 되지만, `CLI`만이 설치되기 때문에 권장하지 않음.

## Installing Docker
- 맥버전은 [여기](https://store.docker.com/editions/community/docker-ce-desktop-mac)에서 다운로드
- 다운받은 `docker.dmg`를 설치해준다. 끝!

## Preference
- General에서 stable/edge 버전간의 switch 가능
- Advanced: docker는 tiny linux vm이기 때문에, CPU, memory 등을 조절가능.
- terminal에서 `docker version`으로 현재 버전을 확인 가능. 나는 `17.11.0-ce` 버전을 설치하였다. 

