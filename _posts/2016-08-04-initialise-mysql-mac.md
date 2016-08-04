---
layout: post
title: "How to initialise MySQL on Mac"
excerpt: ""
date: "2016-08-04"
slug: "initialise-mysql-mac"
categories: blog
tags:
  - MySQL
  - Database
---
Extract the [tar ball](http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.14-osx10.11-x86_64.tar).

Initialise the data directory.

```
bin/mysqld --initialize --user=<your non-root username> --basedir=<where the binaries have been put to> --datadir=<the database directory>
``` 

Watch out for a line of text from the console output like below:

```
[Note] A temporary password is generated for root@localhost: *******
```
Note down the password.

Start the daemon.

```
bin/mysqld_safe --user=<your non-root username> --basedir=/Users/zxu/Library/MySQL/current --datadir=<the database directory> &
```

**Note**: if you hit the following error when starting the daemon on Linux:

```
error while loading shared libraries: libaio.so.1: cannot open shared object file: No such file or directory
```

If probably means that the library in question has not been installed. You can install it like below:

```
sudo apt-get install libaio1 libaio-dev
```

Launch the interactive SQL shell, give it the password you captured before, and change the root password.

```
$ bin/mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.14

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SET PASSWORD = PASSWORD('root');
Query OK, 0 rows affected, 1 warning (0.01 sec)

mysql> exit
Bye
```
 
Shutdown the daemon.

```
bin/mysqladmin -u root -p shutdown
```

