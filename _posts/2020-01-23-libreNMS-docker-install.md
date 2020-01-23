---
layout: post
title: LibreNMS docker install
categories: [blog, net]
tags: [linux, librenms, docker, debian, snmp]
---


BBDD
<pre>
docker run -d \
-v `pwd`/mysql:/var/lib/mysql \
-v `pwd`/50-server.cnf:/etc/mysql/mariadb.conf.d/50-server.cnf:ro \
-e MYSQL_ROOT_PASSWORD=mysqlPASWORD \
--name librenms-db \
mariadb:latest

Lanza librenms, se asocia a la BBDD

docker run -d \
-v `pwd`/data:/data \
-p 888:80 \
-e TZ="Europe/Madrid" \
--link 0bd3a3993581(id container BBDD):mysql \
-e POLLER=24 \
-e DB_TYPE=mysql \
-e DB_HOST=mysql \
-e DB_NAME=librenms \
-e DB_USER=root \
-e DB_PASS=mysqlPASWORD \
--name librenms \
seti/librenms

Al navegador http://ipserver:888/install.php
</pre>
