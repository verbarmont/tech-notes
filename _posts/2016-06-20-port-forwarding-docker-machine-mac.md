---
layout: post
title: "How to forward port for docker-machine on Mac"
excerpt: ""
date: "2016-06-20"
slug: "mac-docker-machine-port-forwarding"
category: 
  - views
  - featured
tags:
  - Mac
  - Docker
show_meta: true
comments: true
mathjax: true
gistembed: true
published: true
noindex: false
nofollow: false
hide_printmsg: false
summaryfeed: false
---

~~~~
machine=default; ssh -i ~/.docker/machine/machines/$machine/id_rsa docker@$(docker-machine ip $machine) -N -L 5432:localhost:5432
~~~~

