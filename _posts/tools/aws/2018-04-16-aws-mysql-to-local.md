---
layout: post
title:  "AWS : dump mysql rds to local mysql"
categories: aws
---


### Download sql from rds to local

Command is simple.

```bash
mysqldump -h rds.host.name -u remote_user_name -p remote_db > dump.sql
```

> Before excute this command, you have to check `rds security group`. Your rds database have to allow `public access` and add your `ip address` for `mysqldump` your sql file.


### Upload sql to local
```bash
mysql -u local_user_name -p local_db < dump.sql
```
