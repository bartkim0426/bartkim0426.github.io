---
layout: post
title: "Install postgres 11 in os x"
categories: postgres
---


```
brew tap petere/postgresql

brew search postgresql

brew install petere/postgresql/postgresql@10
```


**Run this command to start postgres manually**
```
pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
psql postgres
```


