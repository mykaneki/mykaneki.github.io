---
title: "Mysql+docker"
date: 2021-01-07
tags: ["mysql","docker"]
---

1. 版本8.0.19

2. 安装命令

   ```bash
   docker run -p 3306:3306 --name mysql \-v /mydata/mysql/log:/var/log/mysql \-v /mydata/mysql/conf/my.cnf:/etc/mysql/my.cnf \-v /mydata/mysql/data:/var/lib/mysql \-e MYSQL_ROOT_PASSWORD=root \-d mysql:8.0.19
   ```

3. 命令

   ```bash
   docker exec -it mysqlone /bin/bash
   ```

   
