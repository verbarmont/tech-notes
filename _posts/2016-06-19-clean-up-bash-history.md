---
layout: post
title: "How to clean up bash command history"
excerpt: "Steps to clean up bash command history."
date: "2016-06-19"
slug: "clean-up-bash-history"
category: 
  - views
  - featured
tags:
  - Linux
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
export HISTCONTROL=ignorespace # So that any command prefixed with a space will not be written to history
 apt-get clean
 history -c
 rm ~/.bash_history 
 exit
~~~~

