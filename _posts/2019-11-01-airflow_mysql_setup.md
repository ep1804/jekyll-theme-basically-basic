---
layout: post
title:  Airflow setup with mysql workthrough
description: "title:  Airflow setup with mysql workthrough"
modified: 2019-11-01
tags: [conda]
---

{% raw %}

ref: https://docs.conda.io/en/latest/miniconda.html#installing

### Assume

- conda environment for python 3 is ready.

### setup MySQL database `air` and user `air` 

```sql
CREATE DATABASE air;
CREATE USER 'air'@'%' IDENTIFIED BY 'password';
GRANT USAGE,EXECUTE ON *.* TO 'air'@'%';
GRANT ALL PRIVILEGES ON orca_metadata_air.* TO 'air'@'%';
FLUSH PRIVILEGES;
```

### install airflow

```bash
export AIRFLOW_HOME='/user/airflow_home_path'

pip install apache-airflow[mysql]
```

if it complains about mysql_config,

```bash
yum install mariadb-devel
```

### setup some configuration in `airflow.cfg`

```
dags_folder = ...
base_log_folder = ...
default_timezone = ...
executor = ...
sql_alchemy_conn = mysql://air:password@<host>:<port>/air
expose_config = ...
max_threads = ...
```

### run

```bash
airflow initdb
airflow scheduler -D
airflow webserver -D
```

### ref

- https://airflow.apache.org/start.html
- https://airflow.apache.org/howto/set-config.html
- https://docs.sqlalchemy.org/en/13/core/engines.html#mysql

{% endraw %}
