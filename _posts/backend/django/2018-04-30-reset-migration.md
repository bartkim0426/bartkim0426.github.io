---
layout: post
title:  "Django: reset migrations"
categories: django
---


## 1. Remove all migration from project

If you don't have to preserve database or migration history during migration, you simply throwing whole migrations away.


### 1. Remove all migration files

Except the `__init__.py`, (or it's ok to remove `migrations` directory itself) remove all migrate files.

You can use `find` command.

```bash
find . -path "*/migrations/*.py" -not -name "__ini__.py" -delete
find . -path "*/migrations/*.pyc" -delete
```

### 2. Drop current db

When you're using `sqlite`, simply remove `db.sqlite3` file. 

Or using `postgres`, just drop db and create database again.

```postgres
psql=# DROP DATABASE db_name;
psql=# CREATE DATABASE db_name;
```

### 3. Recreate initial migrations and re-generate db schema.

```bash
python manage.py makemigrations
python manage.py migrate
```



## 2. `--fake` zero

If you want to clear migration history app by app and keep the rest migration history, you can use `--fake` command.


### 1. `showmigrations` command to see history
First, see all tracking migrations with `showmigrations`. It will shows you all migrations files.

```bash
python manage.py showmigrations

admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
auth
 [X] 0001_initial
 [X] 0002_alter_permission_name_max_length
 [X] 0003_alter_user_email_max_length
 [X] 0004_alter_user_username_opts
 [X] 0005_alter_user_last_login_null
 [X] 0006_require_contenttypes_0002
 [X] 0007_alter_validators_add_error_messages
contenttypes
 [X] 0001_initial
 [X] 0002_remove_content_type_name
post
 [X] 0001_initial
 [X] 0002_remove_mymodel_i
 [X] 0003_mymodel_bio
sessions
 [X] 0001_initial
```

### 2. clear migration history with `--fake zero`
Clear migration history from `post` app.

```bash
python manage.py migrate --fake post zero
```

Then run `showmigrations` again.


```bash
python manage.py showmigrations

admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
auth
 [X] 0001_initial
 [X] 0002_alter_permission_name_max_length
 [X] 0003_alter_user_email_max_length
 [X] 0004_alter_user_username_opts
 [X] 0005_alter_user_last_login_null
 [X] 0006_require_contenttypes_0002
 [X] 0007_alter_validators_add_error_messages
contenttypes
 [X] 0001_initial
 [X] 0002_remove_content_type_name
post
 [ ] 0001_initial
 [ ] 0002_remove_mymodel_i
 [ ] 0003_mymodel_bio
sessions
 [X] 0001_initial
```

### 3. clear real migration files

Inside target app (`post` app in example), remove all migration files.

```python
find . -path "*/migrations/*.py" -not -name "__init__.py" -delete
find . -path "*/migrations/*.pyc"  -delete
```

Then `showmigrations` again.

```python
python manage.py showmigrations

admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
auth
 [X] 0001_initial
 [X] 0002_alter_permission_name_max_length
 [X] 0003_alter_user_email_max_length
 [X] 0004_alter_user_username_opts
 [X] 0005_alter_user_last_login_null
 [X] 0006_require_contenttypes_0002
 [X] 0007_alter_validators_add_error_messages
contenttypes
 [X] 0001_initial
 [X] 0002_remove_content_type_name
post
 (no migrations)
sessions
 [X] 0001_initial
```

### 4. Create initial migrations and migrate again

```bash
python manage.py makemigrations
python manage.py migrate --fake-initial
```

Then you can see initial migrate.


```bash
python manage.py showmigrations

admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
auth
 [X] 0001_initial
 [X] 0002_alter_permission_name_max_length
 [X] 0003_alter_user_email_max_length
 [X] 0004_alter_user_username_opts
 [X] 0005_alter_user_last_login_null
 [X] 0006_require_contenttypes_0002
 [X] 0007_alter_validators_add_error_messages
contenttypes
 [X] 0001_initial
 [X] 0002_remove_content_type_name
post
 [X] 0001_initial
sessions
 [X] 0001_initial
```
