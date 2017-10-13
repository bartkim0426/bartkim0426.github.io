---
layout: post
title:  "Restart postgres in ubuntu 16.04"
categories: postgres
tag: postgres
---

## How to restart postgresql in ubuntu 16.04?

First, you have to check psql running. check by `pg_lscluseter`    

Then you can restart by few commands. (choose one or try all)


```bash
sudo service postgresql restart
sudo /etc/init.d/postgresql restart
# you should write version & name on result after pg_lscluster
pg_lsclusters 9.5 main restart
```
