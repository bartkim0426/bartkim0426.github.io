---
layout: post
title:  "Django: connect to mysql with django"
categories: django
---


I always use `postgres` when using django. With `PostgreSQL`, django supports many features such as `Full text search`, `Arrayfield`, and so on.


### Install `MySQL` in mac os X

Can install esaily by `brew`.

```bash
brew install mysql
```

### Start `MySQL`

```bash
mysql.server start
```


### Make database and user

First, open mysql shell

```bash
mysql -u root -p
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5
Server version: 5.7.20 Homebrew

Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

Then Make database, and grant previleges.
```mysql
CREATE DATABASE taskbuster_db;
CREATE USER 'username'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON taskbuster_db.* TO 'username'@'localhost';
FLUSH PRIVILEGES;
quit
```

### Configuration in django settings

First, install `PyMySQL`

```bash
pip insatll PyMySQL
```

After that, add `DATABASE` settings in `settings.py`


```python
pymysql.install_as_MySQLdb()

MYSQL_DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'db_name',
        'USER': 'db_user',
        'PASSWORD': 'your_password',
        'HOST': 'localhost',
        'PORT': '3306',
        'OPTIONS': {
            'init_command': "SET SESSION sql_mode = 'NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'",
        },
    }
}

DATABASE = MYSQL_DATABASES
```


