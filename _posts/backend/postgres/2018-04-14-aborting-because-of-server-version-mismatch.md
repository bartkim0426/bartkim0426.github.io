---
layout: post 
title:  "Postgres: pg_dump: aborting because of server version mismatch Error"
categories: postgres
---


I tried to `pg_dump` from `aws ec2` to local postgres. I run command below, and I encountered `version mismatch` error.

```bash
pg_dump -h <public dns> -U <my username> -f <name of dump file .sql> <name of my database>

pg_dump: server version: 9.4.1; pg_dump version: 9.3.6
pg_dump: aborting because of server version mismatch
```

I found out that ubuntu server has `9.3` postgres and ec2 has `9.4`. 

To solve this problem, I have to upgrade ubuntu postgres to `9.4`.


Upgrade `9.3` to `9.4` article is [Here]() in my blog.


After upgrade postgres 9.3 to 9.4 (eqauls to aws postgres), you can `pg_dump` data.


> If you not remove existing `9.3` postgres, your `pg_dump` command version is still `9.3.x`. If so, you can make symbolic link in `/usr/bin`

```bash
sudo ln -s /usr/lib/postgresql/9.4/bin/pg_dump /usr/bin/pg_dump --force
```

