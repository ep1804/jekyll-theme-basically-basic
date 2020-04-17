---
layout: post
title:  Airflow setup with mysql walkthrough
description: "title:  Airflow setup with mysql walkthrough"
modified: 2019-11-01
tags: [airflow]
---

{% raw %}

### Assume

- conda environment for python 3 is ready.

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
(py3) $ export AIRFLOW_HOME=~/airflow
(py3) $ pip install apache-airflow
(py3) $ airflow version
```

if it complains about mysql component, install `mysqlclient`

```bash
yum install python-devel mysql-devel
pip install mysqlclient
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
  # rotate log files weekly
  weekly
  # keep 1 week worth of backlogs
  rotate 1
  # remove rotated logs older than 7 days
  maxage 7
  missingok
}
```

### ref

- https://airflow.apache.org/docs/stable/start.html
- https://docs.sqlalchemy.org/en/13/core/engines.html#mysql
- https://stackoverflow.com/questions/14087598/python-3-importerror-no-module-named-configparser
- https://stackoverflow.com/questions/39073443/how-do-i-restart-airflow-webserver
- https://unix.stackexchange.com/questions/530828/airflow-log-rotation

{% endraw %}
