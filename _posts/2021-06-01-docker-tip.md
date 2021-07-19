---
layout: post
title: Docker tip
description: "Docker tip"
modified: 2021-06-01
tags: [docker]
---

Docker image build/usage tips

### command

build, local tagging

```
cd your-image-directory
DATE=`date "+%Y%m%d"`

docker build --build-arg base_image=repo.com/base-image:latest -t your-image:$DATE .
docker image tag your-image:$DATE your-image:latest
```

deploy

```
docker tag your-image:$DATE repo.com/your-image:$DATE
docker push repo.com/your-image:$DATE

docker tag your-image:$DATE repo.com/your-image:latest
docker push repo.com/your-image:latest
```

### simple Dockerfile example

install basic tools like java, scala, maven, sbt, conda, spark

```
ARG base_image
FROM $base_image

USER root

RUN yum install -y http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-2.noarch.rpm
RUN yum install -y git
RUN yum install -y krb5-devel
RUN yum install -y epel-release
RUN yum install -y gcc

USER your_account
WORKDIR /your_dir

ENV SPARK_VER=spark-2.4.5
ENV SPARK_DST=${SPARK_VER}-bin-without-hadoop

COPY --chown=user_account:user_account ./resource ./install_resource
RUN ./install_resource/setup.sh
RUN mv ./install_resource/.git-prompt-colors.sh .
RUN cat ./install_resource/bashrc_add.sh >> .bashrc
RUN rm -rf ./install_resource

ENV LANG=en_US.UTF-8
ENV SCALA_HOME=/.../opt/scala-2.11.12

ENV PATH=$JAVA_HOME/bin:$PATH
ENV PATH=$SCALA_HOME/bin:$PATH
ENV PATH=/.../opt/sbt/bin:$PATH
ENV PATH=/.../opt/apache-maven-3.6.3/bin:$PATH

ENV PATH=/.../opt/node-v12.16.1-linux-x64/bin:$:$PATH
ENV SPARK_HOME=/.../opt/${SPARK_DST}
ENV SPARK_CONF_DIR=$SPARK_HOME/conf
ENV SPARK_SUBMIT_OPTS="-Dhdp.version=3.1.0.0-78"

COPY --chown=your_account:your_account ./install_resource/spark_jars_add/* ./opt/${SPARK_DST}/jars/

COPY ./entrypoint.sh ./opt/
ENTRYPOINT ["/.../opt/entrypoint.sh"]

CMD ["/bin/bash"]
```

in `./install_resource/setup.sh`

```bash
#!/usr/bin/bash

mkdir -p opt

cd opt || exit

# scala
wget https://downloads.lightbend.com/scala/2.11.12/scala-2.11.12.tgz
tar -zxvf scala-2.11.12.tgz
rm -f scala-2.11.12.tgz

# maven
wget http://apache.mirror.cdnetworks.com/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
tar -zxvf apache-maven-3.6.3-bin.tar.gz
rm -f apache-maven-3.6.3-bin.tar.gz

# sbt
wget https://github.com/sbt/sbt/releases/download/v1.2.8/sbt-1.2.8.tgz
tar -zxvf sbt-1.2.8.tgz
rm -f sbt-1.2.8.tgz

# spark
wget https://archive.apache.org/dist/spark/${SPARK_VER}/${SPARK_DST}.tgz
tar -xvf ${SPARK_DST}.tgz
rm -f ${SPARK_DST}.tgz

cd ..

# git bash prompt
git clone https://github.com/magicmonty/bash-git-prompt.git ~/.bash-git-prompt --depth=1

# sometimes following files should exist for git's ssh action
mkdir -p .ssh
touch .ssh/authorized_keys
touch .ssh/config
touch .ssh/known_hosts
```

in `./install_resource/bashrc_add.sh`

```
export PS1='\[\e[36m\]\u\[\e[0m\]@\[\e[36m\]your-prompt \[\e[36m\]\W \[\e[33m\]$\[\e[0m\] '

export LANG=en_US.UTF-8
export SCALA_HOME=/.../opt/scala-2.11.12

export PATH=$JAVA_HOME/bin:$PATH
export PATH=$SCALA_HOME/bin:$PATH
export PATH=/.../opt/sbt/bin:$PATH
export PATH=/.../opt/apache-maven-3.6.3/bin:$PATH

# bash git prompt
GIT_PROMPT_ONLY_IN_REPO=1
GIT_PROMPT_THEME=Custom
GIT_PROMPT_THEME_FILE=/.../.git-prompt-colors.sh
source ~/.bash-git-prompt/gitprompt.sh

export SPARK_HOME=/.../opt/spark-2.4.5-bin-without-hadoop
export SPARK_CONF_DIR=$SPARK_HOME/conf

alias spark-submit='$SPARK_HOME/bin/spark-submit'

source /.../miniconda/etc/profile.d/conda.sh
conda activate py2
```

in `./entrypoint.sh`

```
#!/usr/bin/bash

export LANG=en_US.UTF-8
export SCALA_HOME=/.../opt/scala-2.11.12
export SPARK_HOME=/.../opt/spark-2.4.5-bin-without-hadoop
export SPARK_CONF_DIR=$SPARK_HOME/conf

export SPARK_SUBMIT_OPTS="-Dhdp.version=3.1.0.0-78 -Dfile.encoding=utf-8"
export OOZIE_TIMEZONE=Asia/Seoul

export PATH=$JAVA_HOME/bin:$PATH
export PATH=$SCALA_HOME/bin:$PATH
export PATH=/.../opt/sbt/bin:$PATH
export PATH=/.../opt/apache-maven-3.6.3/bin:$PATH
export PATH=/.../opt/node-v12.16.1-linux-x64/bin:$:$PATH

export EXTERN_CLUSTER_CONF="-Dipc.client.fallback-to-simple-auth-allowed=true
-Ddfs.nameservices=xxx,..,..
-Ddfs.ha.namenodes.xxx=nn1,nn1
-Ddfs.client.failover.proxy.provider.xxx=org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider
-Ddfs.namenode.http-address.xxx.nn1=...
-Ddfs.namenode.http-address.xxx.nn2=...
-Ddfs.namenode.https-address.xxx.nn1=...
-Ddfs.namenode.https-address.xxx.nn2=...
-Ddfs.namenode.lifeline.rpc-address.xxx.nn1=...
-Ddfs.namenode.lifeline.rpc-address.xxx.nn2=...
-Ddfs.namenode.rpc-address.xxx.nn1=...
-Ddfs.namenode.rpc-address.xxx.nn2=...
-Ddfs.namenode.service.rpc-address.xxx.nn1=...
-Ddfs.namenode.service.rpc-address.xxx.nn2=...

alias spark-submit='$SPARK_HOME/bin/spark-submit'

source /.../opt/miniconda/etc/profile.d/conda.sh
conda activate py2

exec "$@"
```
