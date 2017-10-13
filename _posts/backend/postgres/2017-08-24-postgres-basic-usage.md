---
layout: post
title:  "postgres basic usage-about Role"
categories: postgres
---

## PostgreSQL managing Role


### Checking Role
```bash
# accessing with postgres user
sudo -u postgres psql
# check user
\du
```

### Creating Role
```bash
# create role in command-line
# answer questions after this command
sudo -u postgres createuser --interactive

# create role in psql prompt
# you can add roles in myrole
sudo -u postgres psql
CREATE ROLE myrole;
```

### Alter Role to user 
there's many role in postgres- LOGIN, CREATEDB, CREATEROLE, etc...
```bash
# you can create with some roles to user
CREATE ROLE myrole WITH LOGIN;
# give role after created
ALTER ROLE myrole WTIH CREATEDB;
```

### setting passwords
```bash
# setting passwod 
\password myrole
```

### setting passwords for user
```bash
ALTER USER postgres with password 'password';
```


### give authuization to role
```bash
# give all auth of table to role
GRANT ALL ON tablename TO rolename;

# give all privileges to role
GRANT ALL PRIVILEGES ON DATABASE database TO rolename;
```
