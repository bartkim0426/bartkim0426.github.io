---
layout: post 
title:  "Docker basic commands - Container (docker mastery-02)"
categories: docker
tags: docker
---


## Basic commands
버전 확인 및 information 확인
```
# check docker versions
docker verson
# config value 확인
docker info
```

### 사용 가능한 docker commands 확인
```
# check docker commands
docker
```
결과가 `Management commands`와 `Commands`로 나뉨
- docker management command: 많은 command를 관리하기 위해서 새로운 format(management commands)를 제공
- `docker <command> <sub-command> (options)`처럼 사용 가능
- 예전 방식 (`docker <command> (optinos)`)도 여전히 사용 가능하다.


## Container

### Image vs Container
- image? application we want to run
- container? instance of that image running as a process

### Container run
```
docker container run [OPTIONS] <image_name> [COMMAND] [ARGS...]
docker container run --publish 80:80 nginx
```
- `nginx` 이미지를 docker hub에서 다운로드
- 해당 이미지에서 새로운 컨테이너 시작
- 80포트를 open
- 80포트에 traffic을 route

**background에서 docker**
```
docker container run --publish 80:80 --detach nginx
```

**docker container list**
```
# new way
# only check running container
docker container ls

# all container including stop one
docker container ls -a

# old way
docker ps
```

**Stop docker container**
```
docker container stop <container_id>

# old way
docker stop <container_id>
```
> `container_id`로는 `docker container ls`에서 확인한 `CONTAINER ID`를 넣어 주면 된다. 다 입력할 필요 없고 unique한 값으로 확인만 되게 앞의 몇자리면 넣어주면 됨


**docekr container start & run** 			

- `docker container run`을 하면 무조건 새로 시작,
- `docker container start`를 하면 stop한 process 재실행
- `--name`으로 이름을 줄 수 있음. 따로 부여하지 않으면 자동 생성(unique한 이름이어야 한다.)
```
docker container run --publish 80:80 --detah --name webhost nginx
```


**Check logs**
```
docker container logs <container name>

# couple options available
docker container logs --help

# old way
docker logs
```

**use top command**
```
docker container top <container_name>
```

**check all commands**
```
docker container --help
```

**Remove all containers** 		
- only can remove not-running process 		

```
docker container rm <container_id>
docker container rm 5bc 6ca 8ac

# remove running container
docker container rm -f <container_id>
```

### `docker container run` 자세히 보기
1. locall image cache에서 image를 찾음
2. remote image repository에서 image를 찾음 (default는 Docker Hub)
3. download latest version
4. image를 기반으로 새로운 container 시작
5. give virtual IP address (inside docker engine)
6. open 80 port (`--publish`)
7. start commands in Dockerfile

### docker container vs VM
- Container: Just a Process, nothing like a VM
- `docker container run`이나 `start`를 실행하면 `ps aux`를 했을때 현재 process에 docker container를 볼 수 있다. 즉, VM이 아닌 프로세스로 실행됨


### Multiple containers settings - `top`, `inspect`, `stats`
- nginx (80:80), mysql(3306:3306), httpd(8080:80): all for `--detach`
- mysql: `--env` option (`-e`) in `MYSQL_RANDOM_ROOT_PASSWORD=yes`
```
# docker postgres
docker container run -d --name postgres -p 5432:5432 -e POSTGRES_PASSWORD=tmfcks478 postgres 
# docker mysql
docker container run -d -p 3306:3306 -e MYSQL_RANDOM_ROOT_PASSWORD=true --name mysql mysql
```

### `docker container inspect`
- Can see metadata about container (startup, config, volums, networking, etc...)
- 많은 데이터가 JSON 타입으로 나옴
```
# docker container inspect <container_id or container_name>
docker container inspect nginx
```

### `docker container stats`
- 현재 진행중인 container들의 live performance가 나옴 (초단위로)
- Memory, net I/O, block I/O 등...
```
docker container stats
```


## Container 안에서 Shell 접근하기 (`-it`)
- container 안에 들어가서 명령어를 실행하려면? (ssh를 사용하는 것처럼)

1. `docker container run -it`
- `docker container run -t` (`--tty`): ssh가 하는것처럼 real simulation을 해줌
- `docker container run -i` (`--interactive`): Keep STDIN open

**bash command로 container 접속**
```
$ docker container run -it --name proxy nginx bash
root@f500d0e3403e:/#
# @ 뒤의 숫자가 container id
# exit로 나갈 수 있음
exit
```
> `exit` 명령어로 bash를 나가면, 해당 container도 stop 된 것을 볼 수 있다. (`docker container ls -a`로 확인 가능)
> 이는 해당 container (여기서는 `proxy` container)를 run 할때 마지막에 `bash` COMMAND를 적어서 bash로 실행시켰기 때문. bash를 나가면 해당 container도 stop됨


### bash command로 ubuntu 접속
```
$docker container run -it --name ubuntu ubuntu
# 일반 ubuntu처럼 apt-get 등의 명령어 사용 가능
root@b470cec4b154:/# apt-get update
root@b470cec4b154:/# apt-get install -y curl

# 이것도 마찬가지로 exit하면 container 종료
exit

# docker container ls로 확인해보면 해당 ubuntu container가 없는것을 확인가능
```

위에서 만든 `ubuntu` container에는 `curl`이 설치되어있다. 하지만 같은 `ubuntu` 이미지로 다른 container를 실행시키면 위 `ubuntu` 컨테이너에 설치한 curl은 없을 것이다. 그럼 이 만들어진 이미지를 다시 실행시키려면? `run`이 아닌 `start` command를 사용하면 됨


**docker container start로 멈춘 (stop or exited) container 재실행**
```
# docker container start --help 로 start 명령어의 옵션을 볼 수있음
# -a (--attach)
# -i (--interactive)
docker container start -ai ubuntu
root@b470cec4b154:/# curl google.com
# curl을 깔지 않았는데도 작동하는 것을 볼 수 있다.
```

**docker exec**
이미 running 되고 있는 container의 shell에 접근하려면? `exec` 명령어를 사용하면 된다.
```
# docker container exec [OPTIONS] <continaer_name or id> COMMAND [ARG...]
# postgres container를 bash COMMAND로 실행
$docker container exec -it portgres bash
root@c1de1d499611:/# su - postgres
$psql
> postgres
# 정상적으로 postgres가 작동함을 볼 수 있다.

# exec 명령어로 bash에 들어왔기 때문에 exit로 나가도 docker container가 종료되지 않음
exit
$docker container ls
```

### docker에서 Alpine 구동

> Alpine? 아주 작은 (거의 5MB) 밖에 하지 않는 linux system


```
# docker hub에서 이미지 가져오는 명령어
$docker pull alpine
# docker image ls 명령어로 현재 있는 image들을 확인 가능
$docker container run -it alpine bash

$docker container run -it alpine bash
docker: Error response from daemon: OCI runtime create failed: container_linux.go:295: starting container process caused "exec: \"bash\": executable file not found in $PATH": unknown.

# `alpine` container를 run 하면 오류가 발생 - alpine은 너무 작아서 bash가 없음
# sh로 실행시켜준다.
$ docker container run -it alpine sh
/ #

# 이후 alpine package 명령어인 apk command 사용 가능
```
