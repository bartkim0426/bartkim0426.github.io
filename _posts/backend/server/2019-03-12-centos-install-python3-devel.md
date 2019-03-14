---
layout: post 
title:  "centos에서 uwsgi 설치 오류 해결 (the gcc failed with exit status 1)"
categories: server
---


ubuntu만 사용하다가 centos에서 django 서버를 세팅하던 중 오류가 발생하여 관련 해결책을 기록해둔다.

```
the gcc failed with exit status 1
```


```
# search package in yum
yum search python3 | grep devel

# install the package in list
yum install -y python3-devel.x86_64
```
