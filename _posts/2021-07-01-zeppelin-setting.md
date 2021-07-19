---
layout: post
title: Zeppelin setting
description: "Zeppelin setting"
modified: 2021-07-01
tags: [zeppelin]
---

Zeppelin installation and setting manual

### zeppelin needs java 1.8 update 151+

- install from https://adoptopenjdk.net/
- set `JAVA_HOME` and `PATH`

### build zeppelin from source

- https://zeppelin.apache.org/docs/0.9.0-SNAPSHOT/setup/basics/how_to_build.html

```bash
git clone https://github.com/apache/zeppelin.git
```

```bash
./dev/change_scala_version.sh 2.11
mvn clean package -DskipTests -Pbuild-distr -Pspark-2.4 -Phadoop3 -Pscala-2.11 -Denforcer.fail=false
```

result: `zeppelin-distribution/target/zeppelin-0.9.0-SNAPSHOT.tar.gz`

#### config for spark

in `conf/zeppelin-env.sh` set:
 
```bash
export SPARK_SUBMIT_OPTIONS='--conf spark.driver.extraJavaOptions="-Dhdp.version=3.1.0.0-78 -Dfile.encoding=UTF-8" --conf spark.executor.extraJavaOptions="-Dhdp.version=3.1.0.0-78 -Dfile.encoding=UTF-8" --conf spark.yarn.am.extraJavaOptions="-Dhdp.version=3.1.0.0-78 -Dfile.encoding=UTF-8"'
export ZEPPELIN_SPARK_USEHIVECONTEXT=false
```

in `conf/zeppelin-site.xml` set:
 
```xml
<property>
  <name>zeppelin.server.addr</name>
  <value>0.0.0.0</value>
  <description>Server binding address</description>
</property>
```

spark interpreter setup (example):

```
SPARK_HOME	/your/spark/home
master	yarn-cluster
spark.app.name	Zeppelin-app
spark.driver.cores	4
spark.driver.memory	16g
spark.executor.cores	4
spark.executor.memory	16g
spark.jars	/your/file/to/include.jar
zeppelin.spark.useHiveContext	true
zeppelin.spark.printREPLOutput	true
zeppelin.spark.maxResult	1000
zeppelin.spark.enableSupportedVersionCheck	true
zeppelin.spark.ui.hidden	false
spark.webui.yarn.useProxy	false
zeppelin.spark.scala.color	true
zeppelin.spark.deprecatedMsg.show	true
zeppelin.spark.concurrentSQL	false
zeppelin.spark.concurrentSQL.max	10
zeppelin.spark.sql.stacktrace	true
zeppelin.spark.sql.interpolation	false
PYSPARK_PYTHON	python
PYSPARK_DRIVER_PYTHON	python
zeppelin.pyspark.useIPython	true
zeppelin.R.knitr	true
zeppelin.R.cmd	R
zeppelin.R.image.width	100%
zeppelin.R.render.options	out.format = 'html', comment = NA, echo = FALSE, results = 'asis', message = F, warning = F, fig.retina = 2	
zeppelin.kotlin.shortenTypes	true
spark.yarn.queue	batch	
spark.serializer	org.apache.spark.serializer.KryoSerializer	
spark.executor.instances	2	
spark.executor.heartbeatInterval	30s	
SPARK_DRIVER_MEMORY	16g	
spark.executorEnv.LANG	en_US.UTF-8	
spark.yarn.appMasterEnv.LANG	en_US.UTF-8
```
