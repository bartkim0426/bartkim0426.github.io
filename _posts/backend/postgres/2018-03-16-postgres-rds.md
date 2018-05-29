---
layout: post 
title:  "RDS에서 로컬로 postgres pg_dump해서 데이터 가져오기"
categories: postgres
---

### 데이터 가져오기

이전에 한번 RDS에서 postgres 데이터를 가져오는 법을 정리한 적이 있는데,
이번에 한 번 더 해보니 잘 되지 않아서 다시한번 트러블슈팅을 기록해둔다.


우선 이 명령어로 postgres 데이터를 가져올 수 있다.

```bash
$ pg_dump -h <public dns> -U <my username> -f <name of dump file .sql> <name of my database>
```

`public dns`에는 ip dns나 `rds endpoint`를 적어도 무방하다.

이렇게 했더니 다음 오류가 발생.

```bash
pg_dump: [archiver (db)] connection to database "gcf" failed: could not connect to server: Connection timed out
        Is the server running on host "***********" (18.195.59.***) and accepting
        TCP/IP connections on port 5432?
```

생각해보니 내가 rds에 해당 ip를 `inbound/outbound`에 등록해 놓지 않았다는게 생각나서 rds에 물린 `security group`에 inboud/outbound에 모두 5432 포트로 내 ip (`125.129.***.***/32`)를 등록해 주었다.


그러고 나니 정상적으로 잘 다운로드 되었다. (`password:`를 입력하는 창이뜨면 성공한 것이다. 데이터의 양이나 rds 리젼에 따라서 시간이 상당히 오래 걸리니 인내심을 가지고 기다리자.)



### 데이터 밀어넣기
다음 명령어로 데이터를 넣을 수 있다.

```bash
$ psql -U <postgresql username> -d <database name> -f <dump file that you want to restore>
```

sql 데이터를 넣던 중, `role`이 없다는 오류가 발생.

```bash
psql:gcf.sql:18859: ERROR:  role "rdsadmin" does not exist
REVOKE
```

psql에 접속해 보니 해당 user가 없었따.
```bash
$psql
postgres# \du

  Role name  |                         Attributes                         | Member of
-------------+------------------------------------------------------------+-----------
 djangodb    | Create DB                                                  | {}
 postgres | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
```

해당 오류를 없애기 위해 `rdsadmin` role을 추가해주었다.


롤을 추가하는 명령어
```bash
sudo -u postgres createuser --interactive
# 만약 현재 접속중인 user가 createuser 권한이 있다면 다음 명령어만 입력해도 무방
# createuser --interactive
```

롤을 추가했다면 rds의 role(user), db name과 맞춰서 db를 생성해준다.
```bash
$psql
postgres# CREATE DATABASE db_name;
postgres# GRANT ALL PREVILIGIES ON DATABASE db_name TO role_name;
```
롤을 추가했다면 다시 sql 데이터를 넣어준다. 

```bash
$ psql -U <postgresql username> -d <database name> -f <dump file that you want to restore>
```

정상적으로 완료가 되었다면 성공적으로 데이터가 들어간 것이다.


### local에서 runserver
`localhost`에서 runserver를 하려면 해당 rds db의 user, db_name, password를 모두 맞춰서 runserver를 해주어야 한다.
