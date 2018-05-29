---
layout: post 
title:  "Docker - Container lifetime"
categories: docker
tags: docker container
---


## Container Lifetime
- Container: immutable infrastructure - 컨테이너는 그 자체로 변하지 않는다: 만약 서비스가 업그레이드되거나 수정되면, 새로운 컨테이너를 만들면 됨! "Only re-deploy, never change"
- db나 unique data들은? 이상적으로는 docker는 이런것들은 가지고 있으면 안됨. => "Seperation of concerns"
- 영구저장이 필요한 데이터들은 컨테이너 내부가 아니라 외부 데이터 저장소를 이용해서 저장해야함.
- "persistent data" => 2가지 솔루션- "Volumes and Bind Mounts"
1. Volumns: make special location outside of container UFS(Union File System) 
> UFC(Union File System)이란?
> 이미지에 변경된 내용(diff)을 추가하는 방식

2. Bind Mounts: link container path to host path


### Persistent Data: Volumes
mysql, postgres 등 db Dockerfile들은 db를 관장하기 때문에 대개 Volume을 사용한다고 한다. 그래서 mysql, postgres를 봐보았다.

- 예시로 [mysql Dockerfile(8.0)](https://github.com/docker-library/mysql/blob/master/8.0/Dockerfile)을 보면 아래쪽에 `VOLUME /var/lib/mysql`을 볼 수 있음.

- [postgres Dockerfile (10)](https://github.com/docker-library/postgres/blob/master/10/Dockerfile)을 봐도 126번째 줄에 `VOLUME /var/lib/postgresql/data`를 볼 수 있다.

container가 실행될 때 `VOLUME` directory를 새로 만들어서, 이 안에 들어오는 모든 파일을 지워지지 않게 함 => container가 지워진다고 지워지지 않고, manually로 지워야함


### MYSQL volume 확인하기
```
# run mysql container 
$docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysql

# check mysql volumes
# Mounts, Volumes 내용을 확인 가능
$docker container inspect mysql

		...
        "Mounts": [
            {
                "Type": "volume",
                "Name": "64a7090d6c0fa197c3546ed9b40642e382ef39fd27bb3af3bbec1fa518fb45db",
                "Source": "/var/lib/docker/volumes/64a7090d6c0fa197c3546ed9b40642e382ef39fd27bb3af3bbec1fa518fb45db/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
		...
            "Volumes": {
                "/var/lib/mysql": {}
            },
			...
```

### Check volumes

```
# check volume
$docker volume ls

DRIVER              VOLUME NAME
local               05db117c75d72d9f5dd63cbd6366f3d5bac682fa1b1878941a836d3dff4fa517

# check volume inspect
$docker volume inspect 05db117c75d72d9f5dd63cbd6366f3d5bac682fa1b1878941a836d3dff4fa517
[
    {
        "CreatedAt": "2017-12-13T06:32:42Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/f306a9e575236b4b4d64d76ee2ea5babfebf1e39e2d28d348c7c84ad85a7bb5f/_data",
        "Name": "f306a9e575236b4b4d64d76ee2ea5babfebf1e39e2d28d348c7c84ad85a7bb5f",
        "Options": {},
        "Scope": "local"
    }
]
```
- `Mountpoint`를 확인 가능하다. linux를 쓰고 있으면 해당 위치에 접근 가능하지만 mac, window는 불가능 (linux vm)
- volume이 어떤 container에 연결되어있는지 등의 정보가 없음: 사용자 친화적이 아님

- running되고있는 container를 지워도 해당 volume은 남아 있음
```
$docker container rm -f mysql mysql2

$docker volume ls

DRIVER              VOLUME NAME
local               1f6dfc0e12eb17c9c29627e25f434c257e0027dd77f1f92c283f3f3a60440575
local               e05ac7319e6034b5bd3622b334c58238287c27fd6ebc52e70308ea796ae39b28
```

=> 이를 위해 `named volumes`가 존재

**using named volumes** 				
```bash
# -v <volume_name>:<volume_path>로 지정 가능
$docker container run -d --name mysql -v mysql-db:/var/lib/mysql mysql

$docker volume ls
DRIVER              VOLUME NAME
local               1f6dfc0e12eb17c9c29627e25f434c257e0027dd77f1f92c283f3f3a60440575
local               e05ac7319e6034b5bd3622b334c58238287c27fd6ebc52e70308ea796ae39b28
local               mysql-db
```

=> named volume을 사용하면 사용자-친화적인 volume 이름 뿐 아니라 mount source가 사용자 친화적인 path로 지정된다.


#### docker volume create
특별한 상황에서 `container run`을 하기 전에 다른 드라이버를 사용해서 volume을 만들고 싶을 때 사용하면 됨.


## Bind Mounting
1개는 volume을 지정하지 않고, 한개는 volume을 지정한 2개의 nginx를 띄워서 mounting을 비교

#### `Dockerfile`
```
# this same shows how we can extend/change an existing official image from Docker Hub

FROM nginx:latest
# highly recommend you always pin versions for anything beyond dev/learn

WORKDIR /usr/share/nginx/html
# change working directory to root of nginx webhost
# using WORKDIR is prefered to using 'RUN cd /some/path'

COPY index.html index.html

# I don't have to specify EXPOSE or CMD because they're in my FROM
```
#### `index.html`
```
<h1>Nginx second template</h1>
```


#### docker run command
```
# $(pwd)를 사용해 현재 경로를 줌
$docker container run -d -p 80:80 --name nginx1 -v $(pwd):/usr/share/nginx/html nginx

$docker container run -d -p 8080:80 --name nginx2 nginx
```
=> 이제 `localhost`와 `localhost:8080`을 각각 들어가보면 `-v`로 볼륨을 지정한 container 에서만 위에서 만든 html이 나옴.

#### check the location
위의 `nginx`의 volume과, 현재 디렉토리 (`pwd`)는 같은 위치이다. 현재 디렉토리에 파일을 추가하면 nginx 내부에 매핑된 디렉토리에도 생기는것을 볼 수 있따.
```
$touch textme.txt
$echo "second file" > textme.txt

$docker container exec -it nginx bash

# I can see 
root@57d498f50094/ cd /usr/share/nginx/html
root@57d498f50094:/usr/share/nginx/html# ls
Dockerfile  index.html  testme.txt

```

이제 `localhost/testme.txt` 에 접속하면 `second file`이 출력되는 것을 볼 수 있다.


### Assignment: volumes
- db upgrade with container
- create postgres: 9.6.1  with named volume
- check docker hub to learn Volume
- check logs, stop container
- new postgres 9.6.2 with named volume (same)
- check logs to validate

1. [ docker hub official postgres](https://hub.docker.com/_/postgres/)에 들어가서 postgres volume 확인 (tag 란에서)

2. psql container run



```bash
$docker container run -d -n psql -v psql:/var/lib/postgres/data postgres:9.6.1

# check log with -f(--follow)
$docker container logs -f psql
```


### Assignment: Bind Mounts
- jekyll: (static site generator) to start local web server

```
$docker container run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve
```

=> 추후에 [deploy jekyll blog with docker]로 정리
