---
layout: post
title:  "Docker : connect to ruuning container by using ssh"
categories: docker
---


You can connect docker container in many ways.

Most simple way is using `docker exec` command.

```
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

Also, you can use `docker attach` command too.

```
docker attach <container-name>
```


But you can use `ssh` for connecting running container.


First, you have to know IP address of the container. You can know by this command

```
# You can know instance_name by
docker container ls

# And add your instance name below
export INSTANCE_NAME="your_instance_name"
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $INSTANCE_NAME
172.17.0.2
```

Then you can connect docker container by

```
ssh root@172.17.0.2
```
