---
layout: post
title: "A better way to scale the Great Firewall of China"
excerpt: "My attempt at circumventing China's Internet censorship."
date: "2016-06-13"
slug: "circumvent_gfw"
categories: blog
tags:
  - Internet
  - China
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

I have been trying to find a way to circumvent China's Internet censorship, or GFW (the Great Firewall), or simply the *Wall* as it's often called. Chinese netizens call this practice *scaling the wall*, or *翻墙*.

Until recently I have been using a commercial VPN service called ExpressVPN that costs 12 US dollars a month. It works in most cases, but very often it takes numerous re-tries to connect to their servers, and the connection gets dropped constantly.

Then one day I checked out the mighty [Zhihu](https://www.zhihu.com) (知乎) website which is the Chinese equivalent to Quora. Some guy suggested that due to the increasingly sophisticated cyber arsenals of Chinese government, almost all conventional VPN services are disrupted to different extends. That's what I have suspected as well. But what's the counter act aside from just putting up with it? They said that a specific tool called Shadowsocks still works fairly well because of its very nature that makes it hard for the censorship apparatus to detect and disrupt.

During my research I came across the 泡泡 website which dedicates a section to teaching people skills to "jump the wall". It has [a nice page](https://pao-pao.net/article/480) with clear instructions on setting up Shadowsocks and that's what I have followed to set up mine.

# Get a machine

First, you need a machine, or a *VPS*, to run the Shadowsocks server. You can get it from one of the following providers:

 - AWS
 - The famous [Digital Ocean](https://www.digitalocean.com) or [Bandwagon Host](https://bandwagonhost.com)
 - Some other less famous (but maybe cheaper) providers such as [HostUs](https://hostus.us) and [Vultr](https://www.vultr.com).

The idea is to get a host that is geographically close (to China), and cheap, of course.

I started with the cheapest host from Digital Ocean. It's located in Singapore and costs $5 per month, which sounds a reasonable price. Without much trouble I set up the Shadowsocks server and successfully connected to it from the clients. But the problem is that the connection is rather flaky. Sometimes it couldn't connect at all. I suspect that it's because the communal IP segments have been added to some sort of blacklist of the Chinese government. If that's the case, it means that other providers may have similar issues as well. Therefore, I turned to AWS to which the connection is harder to be blocked completely by the government.

In AWS' Singapore region I launched a `t2.micro` Ubuntu instance that I intend to keep running. (A rough calculation indicates that the monthly cost would be less than $20.)

To enable the necessary inbound network traffic to the AWS container, I opened up the default SSH port (21) and the Shadowsocks port (8388) in its security group.

# Set up the server
Then you roll up your sleeves to set up the Shadowsocks server on the machine. For this part I followed *almost* exactly the steps in [pao-pao.net page.](https://pao-pao.net/article/480).

I said *almost* because there's just one catch which I will mention shortly, but now let's see the major steps:

~~~~
ssh -i "your_key_pair.pem" ubuntu@public-host-name-of-your-EC2-machine
~~~~
Now you are in the EC2 machine.

~~~~
sudo su -
python # to make sure Python is of the right version.
apt-get update
apt-get install build-essential python-pip python-m2crypto python-dev
pip install gevent shadowsocks
~~~~
Now you have installed Shadowsocks. It's time to configure it and start it up.

Create a configuration file as below:

~~~~
{
    "server":"<IP address>",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"<password>",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false,
    "workers": 1
}
~~~~
Here comes the catch I mentioned earlier. For the IP address, you should give it the **private** IP address rather than the public one; Otherwise, the server won't be able to start.

Once the configuration file is there, try to start the server manually as below:

~~~~
ssserver -c /etc/shadowsocks.json
~~~~

If it looks all right, then you can proceed to make it start automatically when the operating system starts, via `supervisor`.

## Configure `supervisor`
If it's not installed, then install it.

~~~~
apt-get install supervisor
~~~~
Then create its configuration file at `/etc/supervisor/conf.d/shadowsocks.conf`:

~~~~
[program:shadowsocks]
command=ssserver -c /etc/shadowsocks.json
autorestart=true
user=nobody
~~~~

Then append the following line to the file `/etc/default/supervisor`:

~~~~
ulimit -n 51200
~~~~

Start the `supervisor` service and load its configurations:

~~~~
service supervisor start
supervisorctl reload
~~~~

Now you can see if everything works as expected by inspecting its log:

~~~~
supervisorctl tail -f shadowsocks stderr
~~~~

# Client configurations

Now that the server is up and running, it's time to see if it indeed helps with the courageous wall scaling act. You need to connect a *client* to the server to find out.

## Mac client

Download it [here](https://github.com/shadowsocks/shadowsocks-iOS/releases).

Fill out the server IP address, port number, password and other stuff as found in the server configuration file, and then you should be good to go. No need to install browser extensions as the Pao-pao page suggests.

## Android

Search for Shadowsocks in Google Play Store and fulfil similar configurations.
