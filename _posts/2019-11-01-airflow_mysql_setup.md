---
layout: post
title:  Airflow setup with mysql walkthrough
description: "title:  Airflow setup with mysql walkthrough"
modified: 2019-11-01
tags: [airflow]
---

{% raw %}

### Assume

- all python and pip commands should be executed in python 3 environment

### setup MySQL database `air` and user `air` 

```sql
CREATE DATABASE air;
CREATE USER 'air'@'%' IDENTIFIED BY 'password';
GRANT USAGE,EXECUTE ON *.* TO 'air'@'%';
GRANT ALL PRIVILEGES ON air.* TO 'air'@'%';
FLUSH PRIVILEGES;
```

### install airflow

Download basic component and run once

```bash
export AIRFLOW_HOME=~/airflow
pip install apache-airflow
airflow version
```

if it complains about mysql component, install `mysqlclient`

```bash
yum install python-devel
yum install mysql-devel
pip install mysqlclient
```

if it complains about mariadb version conflict, unstall mariadb [ref](https://stackoverflow.com/a/41057656/3243108)

```bash
sudo yum -y remove mariadb-libs
```

if it complains about HTMLString, install `wtforms 2.2.1` [ref](https://stackoverflow.com/a/61364347/3243108)

```bash
pip install 'wtforms==2.2.1'
```

for docker operation, install `docker python`

```bash
pip install docker
```

### setup some configuration in `airflow.cfg`

```
dags_folder = ...
base_log_folder = ...
default_timezone = ...
executor = LocalExecutor
sql_alchemy_conn = mysql://air:password@<host>:<port>/air
expose_config = ...
max_threads = ...
```

### run

initialize db

```bash
airflow initdb
```

run scheduler and webserver

```bash
airflow scheduler -D
airflow webserver -p 8280 -D
```

### kill and reset

at airflow home,

```bash
cat *.pid | xargs kill
rm *.pid
airflow resetdb
```

### setup logrotate

e.g. add file `/etc/logrotate.d/airflow`

```
/<log-path>/*/*.log /<log-path>/*/*/*.log /<log-path>/*/*/*/*.log /<log-path>/*/*/*/*/*.log {
  # rotate log files daily keeping 1 week worth of backlogs
  daily
  rotate 7
  maxage 7

  # set extension
  dateext # default
  dateyesterday

  # logrotate is picky about permission
  su [username] [username]

  # safety measures
  missingok
  notifempty
  copytruncate
}
```

### maintenance dags

- https://github.com/teamclairvoyant/airflow-maintenance-dags

### ref

- https://airflow.apache.org/docs/stable/start.html
- https://docs.sqlalchemy.org/en/13/core/engines.html#mysql
- https://stackoverflow.com/questions/14087598/python-3-importerror-no-module-named-configparser
- https://stackoverflow.com/questions/39073443/how-do-i-restart-airflow-webserver
- https://unix.stackexchange.com/questions/530828/airflow-log-rotation

{% endraw %}
