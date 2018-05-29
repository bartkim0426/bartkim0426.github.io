---
layout: post
title:  "nosql: basic usage of OrientDB"
categories: database
---

[`OrientDB`](https://orientdb.com/) is kind of `NoSQL` database. It has several advantages such as `Multi-master replication`, `SQL support` and so on.



### Install

Before install `OrientDB`, you need `Java`. Just go to this [Java Download page](http://www.oracle.com/technetwork/java/javase/downloads/index.html) and download as your os.

After install java, just download `OrientDB` [here](https://orientdb.com/download-2/) and unzip the file.

Go to `orientdb-community-importers-x.x.x/bin/` directory and run `server.sh`

```bash
sudo ./server.sh
```

Or if you're using mac, just easily install by `homebrew`
```bash
brew install orientdb
```

### Start

```bash
sudo orientdb start
```


