---
layout: post
title:  "accessing other server postgres"
categories: postgres
---

## Access other server postgres

### In postgres server

1. finding `pg_hab.conf`    

you can find with next command
```bash
# login with sudo user
# you can skip but you have to write sudo command
sudo -s

# finding pg_hba
find / -type f -name "pg_hba.conf"
```

2. edit setting in `pg_hab.conf`
```bash
# go to pg_hab.conf dir
# it's different between psql install dir
cd /etc/postgresql/9.3/main
# open pg_hba.conf
vim pg_hba.conf
```
**Add following line in ip address of client machine**
```
host all all x.x.x.x/24 trust
```

3. edit `postgresql.conf`
```bash
vim postgresql.conf
```
**Add wildcardw in listen address**
> maybe there's already listen_address setting and you have to find with / and change to next setting
```bash
listen_addresses = '*'
```

4. restart postgresql
```bash
# you can use other command to restart
sudo service restart postgresql
```
