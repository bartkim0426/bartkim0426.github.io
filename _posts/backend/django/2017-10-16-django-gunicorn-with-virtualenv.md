---
layout: post
title:  "django deploy with gunicorn, virtualenv"
categories: django
tag: django gunicorn virtaulenv
---

## 문제
gunicorn 명령어로 gunicorn을 실행하면 잘 되는데, gunicorn.conf를 통해서 gunicorn을 실행하면 잘 되지 않는다. 

pyenv 문제인 것 같아서 virtualenv로 실생시켰는데 동일한 문제 발생

## 해결책
일단 --daemon으로 background에서 실행시킴
```bash
~/myenv/bin/gunicorn --workers 3 --bind unix:/home/soma/gcf/gcf.sock config.wsgi:application --chdir /home/soma/gcf --daemon
```
이러면 백그라운드에서 실행은 된다. 추후 `init`에서 `gunicorn.conf`로 실행시키는 방법도 연구해봐야 할듯.


> 참고: https://github.com/kimyu92/wc-code-sample/blob/master/README.md
