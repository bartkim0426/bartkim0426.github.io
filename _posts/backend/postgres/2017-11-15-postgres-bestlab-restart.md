---
layout: post
title: "postgres systemctl command"
categories: postgres
tag: postgres systemctl
---

ubuntu 16.04에서 postgres를 사용해보면서 service 명령어가 아니라 systemctl 명령어를 처음 사용해보았다. 그런데 계속 restart, start 등의 명령어로도 postgresql을 찾을 수 없다는 오류가 나와서 글을 찾던 중 SO에서 여러 버전의 postgresql이 깔린 경우에는 해당 버전을 다 적어주어야 한다는 말을 보고 시도해봤다. 결과는 성공!


> postgresql 9.5를 기준으로 작성한 명령어


```
sudo systemctl restart postgresql@9.5-main.service
```
