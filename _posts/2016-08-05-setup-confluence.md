---
layout: post
title: "How to set up Confluence"
excerpt: ""
date: "2016-08-05"
slug: "setup-confluence"
categories: blog
tags:
  - Confluence
---
Extract the Confluence tar bar.

[Set the Confluence home directory](https://confluence.atlassian.com/doc/confluence-home-and-other-important-directories-590259707.html).

Prepare the MySQL database:

```
mysql> CREATE USER 'confluence'@'localhost' IDENTIFIED BY 'confluence' PASSWORD EXPIRE NEVER;
Query OK, 0 rows affected (0.02 sec)

mysql> CREATE DATABASE confluence CHARACTER SET utf8 COLLATE utf8_bin;
Query OK, 1 row affected (0.01 sec)

mysql> GRANT ALL PRIVILEGES ON confluence.* TO 'confluence'@'localhost';
Query OK, 0 rows affected (0.01 sec)
```

[Download](http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.14-osx10.11-x86_64.tar) and [install](https://confluence.atlassian.com/doc/database-setup-for-mysql-128747.html#DatabaseSetupForMySQL-Step6.DownloadandinstalltheMySQLdatabasedriver) the MySQL JDBC driver.

Start Confluence:
```
bin/start-confluence.sh
```

When it comes to setting up the JDBC URL, [remove the storage-engine session variable](https://confluence.atlassian.com/confkb/confluence-fails-to-start-with-error-unknown-system-variable-storage_engine-using-mysql-5-7-x-789090576.html), and append the ```useUnicode``` and ```characterEncoding``` query parameters:

```
jdbc:mysql://localhost/confluence?sessionVariables=&useUnicode=true&characterEncoding=utf8
```
