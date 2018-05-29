---
layout: post
title:  "Postgres - how to dump sql from Amazon RDS to local postgres?"
categories: postgres
---
Change your database RDS instance security group to allow your machine to access it.


Add your ip to the security group to acces the instance via Postgres.

Make a copy of the database using pg_dump

```bash
$ pg_dump -h <public dns> -U <my username> -f <name of dump file .sql> <name of my database>
```

you will be asked for postgressql password.

a dump file(.sql) will be created


Restore that dump file to your local database.


but you might need to drop the database and create it first

```bash
$ psql -U <postgresql username> -d <database name> -f <dump file that you want to restore>
```
the database is restored



#####ref

http://stackoverflow.com/questions/31881786/how-to-pg-dump-an-rds-postgres-database


http://www.thegeekstuff.com/2009/01/how-to-backup-and-restore-postgres-database-using-pg_dump-and-psql/
