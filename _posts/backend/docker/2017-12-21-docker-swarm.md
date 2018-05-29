---
layout: post 
title:  "Docker - swarm"
categories: docker
tags: docker swarm
---

## containers everywhere = New problems
- how do we automate container lifecycle?
- how can we easily scale uot/in/up/down?
- re-created if fail
- blue/green deploy?


## Swarm mode: built-in orchestration
- swarm: 2016년에 나옴. 
- server clustering solution
- not related to swarm classic for pre-1.12

### 기본 개념
- Manager & Worker 개념
- Manager: Raft consensus group (role) - worker에 manager role이 부여된 것...
- new `docker service` 커맨드: 몇개를 run 할건지 등을 정할 수 있다! 



## Swarm mode 써보기
우선 `swarm mode`가 active인지 확인해보아야 한다.
```
$docker info
...
Swarm: inactive
...
```
=> default로 inactive로 되어있음
```
$docker swarm init
Swarm initialized: current node (wj5jyako6ytilukz7lv4zbtmj) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-4yizvphkosvyrg6m3dal6l86ns4wdw5khiewkfwd4ls45o1fci-137fhpaqyyxm3def5msu1uwwm 192.168.65.2:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

$docker info
```
위 명령어로 swarm을 activate 할 수있다.


### Swarm 명령어들
```
# 한개의 Leader가 있는것을 볼 수 있다.
$docker node ls
ID                            HOSTNAME                STATUS              AVAILABILITY        MANAGER STATUS
wj5jyako6ytilukz7lv4zbtmj *   linuxkit-025000000001   Ready               Active              Leader

$docker node --help
$docker swarm --help
$docker service --help
```

### service 명령어
```
$docker service --help

$docker service create alpine ping 8.8.8.8

$docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
gm24sz08zf80        jolly_turing        replicated          1/1                 alpine:latest

# docker service ps <sevice_id or service_name>으로 process id를 조회 가능
# docker container ls 와 비슷한 결과, NODE도 확인 가능
$docker service ps jolly_turing
ID                  NAME                IMAGE               NODE                    DESIRED STATE       CURRENT STATE                ERROR               PORTS
rtn0h4nfv10j        jolly_turing.1      alpine:latest       linuxkit-025000000001   Running             Running about a minute ago
```
=> service의 ID는 container process의 id와는 다름. 

### scale up 하기
`docker service update <service_id> --replicas <늘릴 숫자>`로 replicas를 늘려 scale up을 할 수 있다.
```
$docker service update gm24sz08zf80 --replicas 3

$docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
gm24sz08zf80        jolly_turing        replicated          3/3                 alpine:latest

# container를 확인해보면 3개가 도는 것을 확인 가능
$docker service ls jolly_turing
ID                  NAME                IMAGE               NODE                    DESIRED STATE       CURRENT STATE                ERROR               PORTS
rtn0h4nfv10j        jolly_turing.1      alpine:latest       linuxkit-025000000001   Running             Running 8 minutes ago
yoebnrtsouvr        jolly_turing.2      alpine:latest       linuxkit-025000000001   Running             Running about a minute ago
7ugtoxn6ip98        jolly_turing.3      alpine:latest       linuxkit-025000000001   Running             Running about a minute ago

$docker container ls
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                  NAMES
156cb3befd9f        alpine:latest             "ping 8.8.8.8"           2 minutes ago       Up 2 minutes                               jolly_turing.2.yoebnrtsouvr4zkk39s80h11o
17672ac6eb1e        alpine:latest             "ping 8.8.8.8"           2 minutes ago       Up 2 minutes                               jolly_turing.3.7ugtoxn6ip9822en9mc0ty3jr
cb9e45cdc8e8        alpine:latest             "ping 8.8.8.8"           9 minutes ago       Up 9 minutes                               jolly_turing.1.rtn0h4nfv10jdumppsopz90yt
```

### `docker update`, `docker service update` 비교

`docker update`와 `docker service update`의 help command를 보면 service update가 훨씬 많은 명령어를 가지고 있는 것을 볼 수 있다.
- docker update: cpu, memory 등을 각 container별로 지정. stop and starting
- docker service update: service에서 관장하는 여러 개의 process들을 rolling하면서 update => green/blue 형태로 (멈추지 않고) update해줌
- 이 경우, 문제가 생겼을 때 docker service에서 자동으로 REPLICAS를 채워준다. 		

```
$docker container ls
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                  NAMES
c7f6bc45cf09        alpine:latest             "ping 8.8.8.8"           2 hours ago         Up 2 hours                                 jolly_turing.1.ys3siv0ebv9dqa0ov89asamyx
156cb3befd9f        alpine:latest             "ping 8.8.8.8"           2 hours ago         Up 2 hours                                 jolly_turing.2.yoebnrtsouvr4zkk39s80h11o
17672ac6eb1e        alpine:latest             "ping 8.8.8.8"           2 hours ago         Up 2 hours                                 jolly_turing.3.7ugtoxn6ip9822en9mc0ty3jr

$docker container rm -f jolly_turing.1.ys3siv0ebv9dqa0ov89asamyx

# 여전히 service의 REPLICAS는 3/3임을 볼 수 있다.
$docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
gm24sz08zf80        jolly_turing        replicated          3/3                 alpine:latest

# docker service ps로 확인해보면 아까 죽인 container가 exit 되고 새로운 container가 자동으로 붙어있음을 볼 수 있다
ID                  NAME                 IMAGE               NODE                    DESIRED STATE       CURRENT STATE                   ERROR                         PORTS
ascg5051cylm        jolly_turing.1       alpine:latest       linuxkit-025000000001   Running             Ready less than a second ago
ys3siv0ebv9d         \_ jolly_turing.1   alpine:latest       linuxkit-025000000001   Shutdown            Failed less than a second ago   "task: non-zero exit (137)"
yoebnrtsouvr        jolly_turing.2       alpine:latest       linuxkit-025000000001   Running             Running 2 hours ago
7ugtoxn6ip98        jolly_turing.3       alpine:latest       linuxkit-025000000001   Running             Running 2 hours ago
```
=> container create 등을 사용하지 않고 `orchestration` 시스템을 사용 			



