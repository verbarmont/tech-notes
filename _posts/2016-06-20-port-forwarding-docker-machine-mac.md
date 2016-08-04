---
layout: post
title: "How to forward port for docker-machine on Mac"
excerpt: ""
date: "2016-06-20"
slug: "mac-docker-machine-port-forwarding"
categories: blog
tags:
  - Mac
  - Docker
---

~~~~
machine=default; ssh -i ~/.docker/machine/machines/$machine/id_rsa docker@$(docker-machine ip $machine) -N -L 5432:localhost:5432
~~~~

