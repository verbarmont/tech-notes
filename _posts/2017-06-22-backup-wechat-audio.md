---
layout: post
title: "How to backup WeChat audio messages"
excerpt: ""
date: "2017-06-22"
slug: "backup-wechat-audio"
categories: blog
tags:
  - Network
  - China
  - Docker
---
WeChat does not provide any means to export voice messages. For many, this is a big pain because over the years those voice messages could contain important or irreplaceable information and memories. Luckily there's a way to do it by ourselves, thanks to the awesome research work many people, such as [Gabriel B. Nunes](http://kronopath.net/about), have done. This blog post is to record the steps that I have followed to make it work in my environment,  combining, and slightly tweaking sometimes, these people's instructions, so that I can easily repeat them in the future.

## Locate those audio files ##
I've only got an Android phone, and that's the only device I've tried this. Basically those audio files are stored somewhere in the file system of the phone, as `amr` files. What we need to do is to find them, get them out of the phone, and put them to a computer where they can be further processed,  by which I mean converting to some other formats that can be easily played.

Check out this screen recording for the details: <video src="../../images/SVID_20170622_065616.mp4" controls preload></video>
