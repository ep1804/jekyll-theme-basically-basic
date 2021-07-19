---
layout: post
title: HBase setting cheetsheet
description: "HBase setting cheetsheet"
modified: 2018-06-29
tags: [hbase]
---

HBase configuration manual

### server configuration

Some example settings frequently used for very large size HBase.

```
hbase.snapshot.master.timeout.millis  3600000
hbase.snapshot.region.timeout  3600000

hbase.hregion.memstore.block.multiplier 16
hbase.server.keyvalue.maxsize  41943040
hbase.hregion.max.filesize 107374182400
hbase.hstore.blockingStoreFiles 32
hbase.hstore.compaction.max 20
```

```
master cpu: 8
master memory: 22400
HBASE_MASTER_OPTS: "-Xmx20480m -Xms20480m ...
```

```
region server cpu: 8
region server memory: 22400
HBASE_REGIONSERVER_OPTS: "-Xmx20480m -Xms20480m ...
```

### client configuration

```
<property>
  <name>hbase.mapreduce.bulkload.max.hfiles.perRegion.perFamily</name>
  <value>10240</value>
  <final>false</final>
  <source/>
</property>
```

### table configuration

```
disable 'your_table'
drop 'your_table'
create 'your_table',
  {METADATA => {'SPLIT_POLICY' => 'org.apache.hadoop.hbase.regionserver.DisabledRegionSplitPolicy'}},
  { NAME => 'm', VERSIONS => 1, BLOCKSIZE => 8192, COMPRESSION => 'SNAPPY', TTL => 8640000 },
  { NAME => 'c', VERSIONS => 1, BLOCKSIZE => 32768, COMPRESSION => 'SNAPPY', TTL => 8640000 },
  { NUMREGIONS => 14400, SPLITALGO => 'UniformSplit' }

alter 'your_table', { NAME => 'M', TTL => 8640000 }, { NAME => 'C', TTL => 8640000 }
```

### table migration

ref: https://stackoverflow.com/a/26274782/3243108

Export

```bash
bin/hbase \
-Dhdp.version=3.1.0.0-78 \
org.apache.hadoop.hbase.mapreduce.Export \
-Dmapreduce.job.queuename=your_queue \
-Dmapreduce.job.running.map.limit=1600 \
your_table \
hdfs://your_dump_location \
40
```

Import

```bash
bin/hbase \
-Dhdp.version=3.1.0.0-78 \
org.apache.hadoop.hbase.mapreduce.Import \
-Dmapreduce.job.queuename=your_queue \
-Dmapreduce.job.running.map.limit=1600 \
your_table \
hdfs://your_dump_location \
```
