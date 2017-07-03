---
layout: post
title: "How to backup WeChat audio messages"
excerpt: ""
date: "2017-06-22"
slug: "backup-wechat-audio"
categories: blog
tags:
  - China
  - Social-Media
---
WeChat does not provide any means to export audio / voice messages. For many, this is a big pain because over the years, important or irreplaceable information and memories could be embedded in those voice messages. Luckily there's a way to do it by ourselves, thanks to the awesome research work many people, such as [Gabriel B. Nunes](http://kronopath.net/about) and [Karl Chen](https://github.com/kn007), have done.

This blog post is to record the steps that I have followed to make it work in my environment, combining, and slightly tweaking sometimes, these people's instructions, so that I can easily repeat them in the future.

## Locate audio files ##
I've only got an Android phone, and that's the only device I've tried this. Basically the audio files are stored somewhere in the file system of the phone, as `amr` files. What we need to do is to find them, get them out of the phone, and put them to a computer where they can be further processed,  by which I mean converting to some other formats that can be easily replayed.
 
Check out the following screen recording for the details:

<video src="../../images/locate-wechat-audio-small.webm" controls preload></video>

## Converte audio files ##
The next step is to converting those `amr` files into some other format, such as `mp3`. For that, you will need a Linux machine with [FFmpeg](https://ffmpeg.org) installed.
 
These are the set-ups that you need to do:

~~~~
apt-get update
apt-get -y install build-essential
apt-get -y install python2.7
apt-get -y install ffmpeg
apt-get -y install wget
apt-get -y install unzip
 
cd
mkdir -p work/converter
mkdir -p work/audios
cd work/converter
 
wget https://github.com/kn007/silk-v3-decoder/archive/master.zip
unzip master.zip
cd silk-v3-decoder-master/
cd silk/
make
~~~~

Then you just run the conversion script again the `amr` files, like this:
 
~~~~
find ~/work/audios/ -name *amr | xargs -I {} sh ./converter.sh {} mp3
~~~~
 
That’s it!
