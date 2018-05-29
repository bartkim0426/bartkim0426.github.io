---
layout: post 
title:  "Fabric 사용하여 django 배포하기"
categories: server
tags: fabric django
---

그동안 django 프로젝트를 배포하면서 여러 가지 시행 착오들을 많이 겪었다. nginx 웹서버에서 uwsgi나 gunicorn 등을 사용해서 배포를 해 보았는데, 생각보다 설정 해야할 것들이 많고 복잡하여 docker를 알아보던 중, 배포 작업을 자동화해주는 `fabric`을 알게 되었고, 이를 사용해보았다. (docker는 나중에...)

## Install Fabric
설치는 간단하게 pip를 통해서 할 수 있따. 
```bash
pip install fabric
# python3을 사용중인 경우 [ fabric3 ](https://pypi.python.org/pypi/Fabric3)을 사용해야한다.
pip install fabric3
```

## make fabfile
