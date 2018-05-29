---
layout: post 
title:  "Postgres: how to upgrade 9.3 to 9.4 in ubuntu 14.04"
categories: postgres
---



## How to upgrade postgres 9.3 to 9.4 in ubuntu

### 1. Create file `/etc/apt/sources.list.d/pgdg.list`
Make `/etc/apt/sources.list.d/pgdg.list` and add a line below.

```
deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main
```

### 2. `apt-get update`

After saving `pgdg.list`, import repository signing key and update package list.

```bash
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | \
  sudo apt-key add -
sudo apt-get update
```

### 3. Install postgres 9.4

```bash
sudo apt-get install postgresql-9.4
```

Then check `9.4` version well

```bash
psql --version
psql (PostgreSQL) 9.4.17
```

> For more information, check [postgres official docs](https://www.postgresql.org/download/linux/ubuntu/)
