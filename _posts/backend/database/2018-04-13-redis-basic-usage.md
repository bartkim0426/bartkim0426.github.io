---
layout: post
title:  "Basic usage of redis"
categories: database
---


> This information is from [redis online tutorial](http://try.redis.io/). You can try your own in [this site](http://try.redis.io/)

### Set argument

Can set variable with `SET`, and get variable with `GET` command

> after `=>` is not a command, it's result of the command.

```redis
SET server:name 'fido'
GET server:name => "fido"

SET connections 10
GET connections => 10
INCR connections => 11
DEL connections
INCR connections => 1
```


### `EXPIRE` and `TTL`

특정 시간동안에만 해당 키를 exist하게 할 수 있다.

```redis
SET resource:lock "Redis Demo"
EXPIRE resource:lock 120
```

120초 동안에만 `resource:lock`을 사용 가능하다. `TTL` 명령어로 얼마나 남았는지 확인이 가능하다.

```redis
TTL resource:lock => 47
TTL resource:lock => -2
```

`-2` 값은 해당 키가 더이상 유효하지 않다는 말이다. `-1`은 해당 키가 만료되지 않는다는 말.

```redis
SET resource:nolock "Redis Demo 2"
TTL resource:nolock => -1
```


### `List` 관련 명령어

중요한 명령어로는 `RPUSH`, `LPUSH`, `LLEN`, `LRANGE`, `LPOP`, `RPOP`이 있다.

`RPUSH`는 해당 리스트의 오른쪽에, `LPUSH`는 왼쪽에 값을 추가해준다.

```redis
> RPUSH friends "Alice"
> RPUSH friends "Bob"
> LPUSH friends "Sam"
```

`LRANGE`로 리스트의 값을 확인할 수 있다. 처음과 마지막 인덱스를 입력하여야 한다.

```redis
> LRANGE friends 0 -1
1) "Sam"
2) "Alice"
3) "Bob"
```
