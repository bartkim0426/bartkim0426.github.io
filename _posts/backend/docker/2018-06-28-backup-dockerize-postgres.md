---
layout: post
title:  "Docker : backup dockerize postgres (fightpain)"
categories: docker
---


```bash
# in docker container
pg_dump -U postgres -d postgres > fightpain_new.sql

# outside coutainer
sudo docker cp 00fd96745:/fight_.sql .

# at once
# outside container
# not work now
sudo docker exec -t 00fd96745 sudo pg_dump -U postgres -d postgres > dump.sql
```
