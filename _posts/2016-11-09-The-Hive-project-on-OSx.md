---
title: The Hive project on OSx (docker)
layout: post
permalink: /2016/11/09/The-Hive-project-on-OSx/
tags: [HiveProject, Elasticsearch, Homebrew]
---

Some notes to start The Hive Project from Docker with this version of Elasticseach and Java on OSx (at the time of writing) :

* OSx 10.12.1
* Java 1.8.0_112
* Elasticsearch 5.0.0

## Install ES and Java on OSx

### Prepare homebrew with an update 

```bash
brew update
```

### Enable services

The following sections will assume that the services are managed with "brew services", enable this feature with :

```bash
brew tap homebrew/services
```

### Install Java 
```bash
brew cask install java
```

### Install Java 
```bash
brew install elasticsearch
```


## Configuration Elastic Search



Check the elastic settings in brew : `brew info elasticsearch`

Edit `/usr/local/etc/elasticsearch/elasticsearch.yml` as follow :

1. Change the cluster name in line 17 :
```bash
cluster.name: hive
```

2. Then add the following keys to the file :

```bash
network.host: 127.0.0.1
script.inline: on
thread_pool.index.queue_size: 100000 
thread_pool.search.queue_size: 100000
thread_pool.listener.queue_size: 1000
```


**Setting explanation** : It's highly recommended to avoid exposing this service to an untrusted zone, if `ElasticSearch` and `TheHive` run on the same host (and not in a docker), set `network.host` parameter with `127.0.0.1`. 

`TheHive` uses dynamic scripts to make partial updates. Hence, they must be activated using with `script.inline=on`.

The cluster name must also be set, use `hive` for example.

`Threadpool` queue size parameters must be set with higher value (100000) since the default size will get the queue easily overloaded.


**Warning** : the release 5.x provides new key names for the Thread Pool settings ([see details](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-threadpool.html)).

## Start the services 

Start elastic search :

```bash
snoop@snoop ~ $ brew services restart elasticsearch 
Stopping `elasticsearch`... (might take a while)
==> Successfully stopped `elasticsearch` (label: homebrew.mxcl.elasticsearch)
==> Successfully started `elasticsearch` (label: homebrew.mxcl.elasticsearch)
```

Bind the ports to localhost :

```bash
snoop@snoop ~ $ sudo lsof -iTCP -sTCP:LISTEN -n -P
Password:
COMMAND    PID  USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
mtmfs       91  root    3u  IPv4 0xa995648426c68373      0t0  TCP 127.0.0.1:49152 (LISTEN)
mtmfs       91  root    5u  IPv4 0xa995648426c67a7b      0t0  TCP 127.0.0.1:49153 (LISTEN)
Dropbox    335 snoop  110u  IPv6 0xa99564842c949a13      0t0  TCP *:17500 (LISTEN)
Dropbox    335 snoop  112u  IPv4 0xa99564842d081a7b      0t0  TCP *:17500 (LISTEN)
Dropbox    335 snoop  142u  IPv4 0xa99564842d061563      0t0  TCP 127.0.0.1:17600 (LISTEN)
Dropbox    335 snoop  149u  IPv4 0xa99564842d060373      0t0  TCP 127.0.0.1:17603 (LISTEN)
2BUA8C4S2  435 snoop   11u  IPv4 0xa99564842c602563      0t0  TCP 127.0.0.1:6258 (LISTEN)
2BUA8C4S2  435 snoop   12u  IPv6 0xa99564842c949f53      0t0  TCP [::1]:6258 (LISTEN)
2BUA8C4S2  435 snoop   13u  IPv4 0xa99564842c601373      0t0  TCP 127.0.0.1:6263 (LISTEN)
2BUA8C4S2  435 snoop   14u  IPv6 0xa99564842c7c6a13      0t0  TCP [::1]:6263 (LISTEN)
java      3611 snoop  142u  IPv6 0xa99564842c7c64d3      0t0  TCP 127.0.0.1:9300 (LISTEN)
java      3611 snoop  168u  IPv6 0xa995648424c9fa13      0t0  TCP 127.0.0.1:9200 (LISTEN)
```

If the service doesn't start, checks the error there : `/usr/local/var/log/elasticsearch.log`

Start The Hive project :

```bash
snoop@snoop ~ $ docker run --publish 127.0.0.1:9000:9000  --volume /Volumes/Media/Docker:/data certbdf/thehive:latest 
```

Fire a web browser and open the app [http://127.0.0.1:9000](127.0.0.1:9000)


## Stop services 

Stop elastic search :

```bash
snoop@snoop ~ $ brew services stop elasticsearch
Stopping `elasticsearch`... (might take a while)
==> Successfully stopped `elasticsearch` (label: homebrew.mxcl.elasticsearch)
```

Stop Docker container :

```bash
docker ps -a
docker stop <CONTAINER ID>
```