---
layout: post 
title:  "Postgres: 한글 정렬하기"
categories: postgres
---



```bash
$ psql

postgres=# \l

                                      List of databases
     Name     |    Owner    | Encoding | Collate |    Ctype    |      Access privileges
--------------+-------------+----------+---------+-------------+-----------------------------
 template0    | seulchankim | UTF8     | C       | ko_KR.UTF-8 | =c/seulchankim             +
              |             |          |         |             | seulchankim=CTc/seulchankim
 template1    | seulchankim | UTF8     | C       | ko_KR.UTF-8 | =c/seulchankim             +
              |             |          |         |             | seulchankim=CTc/seulchankim
...
```

그냥 db를 생성하면 `Collate`가 변수가 `ko_KR.UTF-8`이나 `en__US.utf8`로 되어있을 것이다. 이를 위에 나와있는 것처럼 `C`로 변경해주면 된다.


### Collate 변경하기

이미 db가 생성이 되어있다면 이를 `pg_dump`를 사용해 백업을 해두고 수정해야한다. (`pg_dumpall`로 모든 데이터를 백업할 수 있다.)

이후 db를 다시 생성해준다.

```bash
$ psql

postgres=# DROP DATABASE <db_name>
postgres=# CREATE DATABASE <db_name> LC_COLLATE 'C';
```


> 만약 tempalte1의 설정때문에 `ERROR:  new collation (C) is incompatible with the collation of the template database (en_US.UTF-8)` 이런 오류가 나면 설정이 없는 template0을 지정해서 생성해준다.

```
postgres=# CREATE DATABASE <db_name> TEMPLATE template0 LC_COLLATE 'C';
```

### Collate C로 쿼리 작성하기

각 쿼리를 뽑을 때마다 collate를 지정해줄수도 있다.


```bash
$ psql

postgres=# SELECT * FROM <table> ORDER BY <field> COLLATE 'C';
```

하지만 매번 쿼리를 작성할 때 이렇게 할 수는 없으므로.. 애초에 Collate C 상태에서 postgres를 설치하면 기본 설정이 collate c가 된다.

### Collate C로 설치하기

postgres를 설치하기 이전에, (혹은 모두 지운 이후에) 환경변수에 `LC_COLLATE`를 넣어주면 `initdb`가 호출되면서 이를 세팅해준다.


```bash
$ export LC_COLLATE="C"
# install postgres
$ brew install postgresql
```
