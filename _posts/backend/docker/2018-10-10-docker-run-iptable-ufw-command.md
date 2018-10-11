---
layout: post
title:  "Docker : run ufw, iptables command in docker container"
categories: docker
---

I tried to use `ufw` and `iptables` in docker container.

However, I got this errors

```
iptables v1.6.1: can't initialize iptables table `filter': Permission denied (you must be root)
Perhaps iptables or your kernel needs to be upgraded.
```


I figured out that I can access with `--privileged` command with `docker exec`.


```
docker exec -it --privileged my_container_id bash

root@my_container_id:/$ sudo ufw status
Status: inactive
```

Also, I can use `--cap-add=NET_ADMIN` when run container.

You can see more detail about capability [here(docker docs)](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities)

```
docker run --cap-add=NET_ADMIN ...
```

