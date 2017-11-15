---
layout: post
title: "postgres 작동 안될 때 list-dependencies 확인하기"
categories: postgres
---

가끔 postgres가 서버에서 정상적으로 작동하지 않을 때 문제를 찾기 위해 로그를 뒤지거나 `systemctl status postgres`로 상태를 보는데, 그것보다 훨씬 쉽고 직관적으로 작동되는 모든 postgres list-dependencies를 확인할 수 있는 명령어가 있어서 남겨놓는다.

```
sudo systemctl list-dependencies postgresql
```
