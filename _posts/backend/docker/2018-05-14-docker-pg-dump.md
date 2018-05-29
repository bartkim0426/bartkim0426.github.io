---
layout: post
title:  "Docker : pg_dump from docker container"
categories: docker
---


### 1. Get bash inside postgres container

```bash
# get name from container
docker container ls

CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS              PORTS                                      NAMES
xxxxxxxxx        xxxxxxxx_webserver   "./start.sh"             2 months ago        Up 2 months         0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   webserver_xxxx
xxxxxxxxx        xxxxxxxx_webapp      "./start.sh"             2 months ago        Up 2 months         8000/tcp                                   webapp_xxxx
xxxxxxxxx        postgres:10.1        "docker-entrypoint.s…"   2 months ago        Up 2 months         5432/tcp                                   db_xxxx

# access bash with name
docker exec -it db_xxxx bash
root@xxxxxxxxx:/# 
```

### 2. pg_dump from psql

```bash
root@xxxxxxxxx:/# pg_dump my_db_name > my_db.sql
```

### 3. copy sql file from container

```bash
# docker cp <container_name>:/<path_to_target_file> <target_directory_path>
docker cp db_xxxx:/my_db.sql .
```

### 4. psql the backup file

```bash
psql -U <user_name> -d <database_name> -f <file_name>
```
