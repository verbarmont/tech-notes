---
layout: post
title: "How to set up Confluence on AWS EC2"
excerpt: ""
date: "2016-11-06"
slug: "setup-confluence-aws-ec2"
categories: blog
tags:
  - Confluence
  - AWS
---
I've decided to move my personal Confluence instance to AWS EC2. This is how I did it.

# Prepare an EC2 instance
I chose the Amazon Linux AMI and the `t2.small` type. The security group I configured allows inbound traffic on the `8090` port.

# Install Docker
```
sudo yum update -y
sudo yum install -y docker
sudo service docker start
```

# Install PostgreSQL
```
docker run --name postgres -p 5432:5432 -e POSTGRES_PASSWORD=<postgres_password> -d postgres:9.5
```
***Note***: Confluence has a bug with the latest PostgreSQL version which is 9.6. That's why I'm using 9.5.

# Set up Confluence database
***Tip***: you can join the PosgreSQL Docker container like this:

```docker exec -it $(docker inspect --format="{{.Id}}" postgres) bash```

Switch to `postgres` then create the role and the database:

```
createuser -S -d -r -P -E confluence
createdb --owner confluence --encoding utf8 <confluence_password>
```

# Install and set up Confluence
Download Confluence installer:

```
curl -L -O https://www.atlassian.com/software/confluence/downloads/binary/atlassian-confluence-5.10.8-x64.bin
```

Install Confluence following Atlassian documentation.

# Make things start automatically

## Start Confluence automatically

Confluence does provide a script to make the Confluence start-up script a UNIX daemon: `bin/install_linux_service.sh`. Just run it as root.

## Start PostgreSQL Docker container automatically
To start the Docker container, I'm using [Supervisor](http://supervisord.org).

Install it as `root`:

```
easy_install supervisor
```
Create a skeleton configuration file:

```
echo_supervisord_conf > /etc/supervisord.conf
```

And add the `program` section:

```
[program:postgres]
command=/home/ec2-user/start-docker.sh
user=ec2-user
```

And the script to start the Docker container is like this:

```
#!/bin/sh

/usr/bin/docker -H unix:///var/run/docker.sock start -i $(/usr/bin/docker inspect --format="{{.Id}}" postgres)
```
***Note***: due to the nature of Supervisor, the Docker container has to be started in the foreground. This is achieved by the `-i` option.

If you want, you can also make Confluence stop the PostgreSQL Docker container when it stops itself. This is achieved by the addition of one line in the Confluence script at `/etc/init.d/confluence`:

```
case "$1" in
    start)
        ./start-confluence.sh
        ;;
    stop)
        ./stop-confluence.sh
	    /usr/bin/docker stop $(/usr/bin/docker inspect --format="{{.Id}}" postgres)
        ;;
    restart)
        ./stop-confluence.sh
        ./start-confluence.sh
        ;;
    *)
        echo "Usage: $0 {start|stop|restart}"
        exit 1
        ;;
esac
```

That's it. :-)