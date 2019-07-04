---
layout: post
title: Manually delete kafka topic workthrough
description: "Manually delete kafka topic workthrough"
modified: 2018-07-04
tags: [kafka]
---

Reference: https://kafka.apache.org/quickstart

### install, list, delete

```bash
wget https://archive.apache.org/dist/kafka/0.10.2.1/kafka_2.11-0.10.2.1.tgz
tar zxvf kafka_2.11-0.10.2.1.tgz
```

```bash
./bin/kafka-topics.sh --zookeeper x.x.x.x:2181/<kafka-root-path> --list
```

```bash
./bin/kafka-topics.sh --zookeeper x.x.x.x:2181/<kafka-root-path> --delete --topic <topic>
```


### manually remove topics in zookeeper

#### run zookeeper client

```bash
wget wget http://apache.tt.co.kr/zookeeper/stable/apache-zookeeper-3.5.5-bin.tar.gz
tar tar zxvf apache-zookeeper-3.5.5-bin.tar.gz
cd apache-zookeeper-3.5.5-bin
./bin/zkCli.sh -server x.x.x.x:2181
```

#### remove topic related nodes

```
deleteall /<kafka-root-path>/brokers/topics/<topic>
deleteall /<kafka-root-path>/admin/delete_topics/<topic>
deleteall /<kafka-root-path>/config/topics/<topic>
```
