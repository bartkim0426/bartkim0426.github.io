---
layout: post
title:  "mysql: 한글 character set UTF-8 설정 (한글이 ???로 나오는 증상)"
categories: database
---


나는 주로 `postgres`를 db로 사용하기 때문에 mysql을 이용해서 db 테이블을 만들고 내용을 넣었을 때 `???`라고 한글이 뜨는 증상을 발견하고 꽤나 당황스러웠다. 

원인을 검색해 보니 `mysql`의 `character set`이 `utf`가 기본이 아니라 발생하는 현상이라고 한다. 이를 `utf-8`로 변경해주면 간단하게 해결 가능하다.


### 해당 table만 변경하기

- 전체 mysql이 아니라 해당 테이블의 데이터만 `utf-8`로 변경 가능하다.

```
mysql> ALTER TABLE table_name CONVERT TO CHARSET utf8;
```


### `my.cnf` 수정하기

- `/etc/my.cnf` (os별로 위치가 다를 수 있다)
- `mysqladmin --help`를 하면 해당 폴더가 어디에 있을지 알려준다.

```
Default options are read from the following files in the given order:
/etc/my.cnf /etc/mysql/my.cnf /usr/local/etc/my.cnf ~/.my.cnf
```

- 해당 파일에 다음 내용을 추가해준다.

```
[client]
default-character-set=utf8

[mysqld]
character-set-server=utf8
collation-server=utf8_general_ci
init_connect=SET collation_connection=utf8_general_ci
init_connect=SET NAMES utf8

[mysql]
default-character-set=utf8
```
