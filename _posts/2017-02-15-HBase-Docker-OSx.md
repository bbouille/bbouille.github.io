---
title: Run HBase in standalone with Docker on OSx  
layout: post
permalink: /2017/02/15/HBase-Docker-OSx/
tags: [HBase, Docker]
---

Some notes to start HBase 1.1.2 in standalone mode with Docker on OSx 10.12.

## Get the hbase-docker


```bash
git clone https://github.com/dajobe/hbase-docker.git
cd hbase-docker
```

### Set the HBase version

Changer HBase version with 1.1.2 in `Dockerfile`: 

```bash
vim Dockerfile
```

### Build the container

```bash
make build
```

### Set the data folder 

```bash
mkdir data
```

## Start HBase

Start the docker container :

```bash
id=$(docker run --name=hbase-docker -h hbase-docker -d -v $PWD/data:/data dajobe/hbase)
```

Check the container and get the Container ID : 
```bash
docker ps
```


## See HBase Logs

If you want to see the latest logs live use:

```bash
$ docker attach $id
Then ^C to detach.
```

To see all the logs since the HBase server started, use:

```bash
$ docker logs $id
and ^C to detach again.
```
