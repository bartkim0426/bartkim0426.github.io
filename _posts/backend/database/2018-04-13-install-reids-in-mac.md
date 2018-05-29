---
layout: post
title:  "Install redis on mac osX"
categories: database
---


You can download `redis` [HERE](https://redis.io/download).

Just download in redis site above, or using `wget`.


```bash
wget http://download.redis.io/releases/redis-4.0.9.tar.gz
tar xzf redis-4.0.9.tar.gz
cd redis-4.0.9
make
```

After `make` command done, just run redis with

```bash
src/redis-server
```

Then you can interact using `redis-cli`

```bash
src/redis-cli
127.0.0.1:6379> set foo bar
OK
127.0.0.1:6379> get foo
"bar"
127.0.0.1:6379>
```


Also, redis provide [`online tutorial`](http://try.redis.io/). If new in `redis`, you can try this.
