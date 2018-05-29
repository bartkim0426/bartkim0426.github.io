---
layout: post
title:  "Mongodb install in mac OS X"
categories: database
---

## Install

Easy installing by homebrew

```bash
brew install mongodb
```


## Make directory for `mongodb`

```bash
sudo mkdir -p /data/db
```

and have to give permission to `/data/db`
```bash
sudo chown $USER /data/db
```

## Basic setting

Activate `mongod`

```bash
mongod
```

And access mongodb shell by `mongo` command

