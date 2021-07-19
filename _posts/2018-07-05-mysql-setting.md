---
layout: post
title: MySQL setting tip
description: "MySQL setting tip"
modified: 2018-07-05
tags: [mysql]
---

### access

```bash
/.../mysql -uroot -p
```

### config database and user

```
CREATE DATABASE rdb_kr;
CREATE USER 'rdb_kr'@'%' IDENTIFIED BY 'rdb_kr';
GRANT USAGE,EXECUTE ON *.* TO 'rdb_kr'@'%';
GRANT ALL PRIVILEGES ON rdb_kr.* TO 'rdb_kr'@'%';
FLUSH PRIVILEGES;
```

