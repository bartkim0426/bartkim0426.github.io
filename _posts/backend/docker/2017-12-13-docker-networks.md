---
layout: post 
title:  "Docker networks"
categories: docker
tags: docker network
---

- Container를 실행시키면 각각의 container가 `bridge`라는 private virtual network에 연결됨
- 각각의 virtual network는 NAT firewall에 라우팅
- 모든 컨테이너는 -p 없이 서로 접속 가능


### docker container의 port 확인하기
```
$docker container port <container_name>
```

### docker container inspect --format
`inspect --format`으로 현재 container의 ip를 확인 가능
```
{% raw %}
docker container inspect --format '{{ .NetworkSettings.IPAddress }}' <container_name>
{% endraw %}
```

## CLI Management for docker network

### `docker network ls`
현재 만들어진 container의 network를 볼 수 있음
```
$docker network ls

NETWORK ID          NAME                DRIVER              SCOPE
538a8d4c6319        bridge              bridge              local
a6c1416c0bcf        host                host                local
d8f50f0bc470        none                null                local
```
> bridge, host, none 세가지 종류가 default로 있음

### `docker network inspect`
현재 network에 연결된 containers, IPAM 등을 확인할 수 있다.
```
docker network inspect bridge
```

### `docker network create --driver`
`docker network create <network_name>` 방식으로 만들면 된다.
```
$docker network create my_new_net
```

### create new network

## Docker Networks: DNS

### container끼리 ping
- `my_nginx`, `new_nginx`라는 두 개의 container를 만들고
- 이를 모두 `my_new_net`에 연결
```
$docker container run -d --name my_nginx --network my_new_net nginx:alpine
$docker container run -d --name new_nginx --network my_new_net nginx:alpine
```
- 이후 서로간에 ping을 보내보면 rely 되어있는 것을 확인 가능하다.

```
$docker container exec -it new_nginx ping my_nginx
$docker container exec -it my_nginx ping new_nginx
```

> DNS server를 default로 가지고 있지 않음
> `create --link`로 만들 수 있지만, `docker-compose`를 이용하면 더 쉽게 만들 수 있다.



## Assignment



### 1. make ubuntu & centos and install curl, compare curl version
```
$docker container --rm -it ubuntu:14.04 bash
root@b488ab9a4e2f:/# apt-get update && apt-get install curl

$docker container -rm -it centos:7 bash
[root@f3e2d5f56921 /]# yum update curl
```
> `--rm` : exit 되었을때 자동으로 container rm


### 2. DNS RR test
- DNS RR test
- DNS Rount Robin Test: make virtual network, create 2 container from elasticsearch:2, research and use `--net-alias search` and give DNS name / use slpine image (nslookup search) with `--net` / use centos curl -s 


1. 새로운 virtual network 만들기 (이름은 무관)
```
$docker network create dude
# 잘 생성되었는지 확인
$docker network ls
```
2. 2개의 elasticsearch container를 위에서 만든 network에서 띄우고 `--net-alias`를 search에 똑같이 물리기
```
$docker container run -d --net dude --net-alias search elasticsearch:2
$docker container run -d --net dude --net-alias search elasticsearch:2
# 잘 생성되었는지 확인
$docker container ls
```

3. `alpine nslookup`으로 search net-alias 검색 
```
$docker contianer run --rm --net dude alpine nslookup search
# search라는 net-alias 명에 위에서 만든 두개의 elasticsearch가 물려 있는걸 확인할 수 있다.
Name:      search
Address 1: 172.19.0.3 elastic2.my_assign
Address 2: 172.19.0.2 elastic1.my_assign
```
> alpine을 만들어 sh로 접속해서 확인해봐도 되지만, 위의 예시처럼 바로 명령어를 때려 넣어서 확인하고 바로 rm해도 된다.

4. `centos curl`로 `search:9200` 포트 확인하기 (elasticsearch 기본 포트카 9200)
```
$docker container run --rm --net dude centos curl -s search:9200
# 다음 결과가 잘 나와야 정상
{
  "name" : "Beautiful Dreamer",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "ulHlYKR4Q9i1p4A_9txRjg",
  "version" : {
    "number" : "2.4.6",
    "build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
    "build_timestamp" : "2017-07-18T12:17:44Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.4"
  },
  "tagline" : "You Know, for Search"
}
```
