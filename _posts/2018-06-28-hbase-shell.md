---
layout: post
title: Shell access to remote HBase workthrough
description: "Shell access to remote HBase workthrough"
modified: 2018-06-28
tags: [hbase]
---

Reference: https://hbase.apache.org/1.2/book.html

### install

```bash
wget https://archive.apache.org/dist/hbase/1.2.6/hbase-1.2.6-bin.tar.gz
tar zxvf hbase-1.2.6-bin.tar.gz
export HBASE_HOME=/path/to/hbase-1.2.6
```

### edit config

```bash
vim $HBASE_HOME/conf/hbase-site.xml
```

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>
  <property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
  </property>
  <property>
    <name>hbase.zookeeper.quorum</name>
    <value>zk1,zk2,zk3</value>
  </property>
  <property>
    <name>zookeeper.znode.parent</name>
    <value>/path/to</value>
  </property>
</configuration>
```

### run shell

```bash
$HBASE_HOME/bin/hbase shell
```

### run script

```bash
#!/usr/bin/env bash

export HBASE_HOME=/path/to/hbase_shell/hbase-1.2.6

vim $HBASE_HOME/conf/hbase-site.xml

$HBASE_HOME/bin/hbase shell
```
