---
layout: post
title: "Docker pull from behind China's Great Firewall"
excerpt: ""
date: "2017-05-13"
slug: "docker-in-china"
categories: blog
tags:
  - Network
  - China
  - Docker
---
Pulling Docker images in China is a pain in the ass - The Great Firewall makes it excruciatingly slow. Fortunately a Chinese company called DaoCloud operates a local Docker registry mirror that can speed things up. Below are the steps to take advantage of this great, and free, service:

### Register an account with DaoCloud
You go to [https://dashboard.daocloud.io](https://dashboard.daocloud.io) to register an account. Probably you also need to verify the email address used for the account.

### Find out the magical URL
Log in to DaoCloud and go to [https://www.daocloud.io/mirror#accelerator-doc](https://www.daocloud.io/mirror#accelerator-doc) where you will find the magics. Particularly, if you are on Mac, you will find the mirror's URL which you are supposed to add to the "Docker machine":

![](../../images/DaoCloud.png?raw=true)

### Update Docker machine

```
$ docker-machine ssh default
                        ##         .
                  ## ## ##        ==
               ## ## ## ## ##    ===
           /"""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
           \______ o           __/
             \    \         __/
              \____\_______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
Boot2Docker version 1.11.0, build HEAD : 32ee7e9 - Wed Apr 13 20:06:49 UTC 2016
Docker version 1.11.0, build 4dc5990
docker@default:~$ sudo sed -i "s|EXTRA_ARGS='|EXTRA_ARGS='--registry-mirror=<the magical URL> |g" /var/lib/boot2docker/profile
docker@default:~$ exit
```

Then restart the docker-machine:

```
$ docker-machine restart default
```

Now when you pull images again, it will fly.