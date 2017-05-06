---
layout: post
title: "How to use NPM with socks proxy on Mac"
excerpt: ""
date: "2017-05-07"
slug: "npm-socks-proxy"
categories: blog
tags:
  - Network
---
NPM only works with HTTP proxy but not socks ones. Hence we need to convert a socks proxy, such as Shadowsocks, to an HTTP proxy. There is a tool on Mac called [polipo](https://www.irif.fr/~jch/software/polipo) for just that.

```
sudo chown root /usr/local/bin/brew
sudo brew install polipo
```

After installing it, you start it like this, assuming your socks proxy is on `localhost:8080`:

```
polipo socksParentProxy=localhost:1080
```

It will then spew out something like below, saying that it now listens on 
the port `8123` as an HTTP proxy server:

```
Established listening socket on port 8123.
```

Now you can tell NPM to use the HTTP proxy that polipo just gave you:

```
npm config set proxy http://127.0.0.1:8123
npm config set https-proxy http://127.0.0.1:8123
```