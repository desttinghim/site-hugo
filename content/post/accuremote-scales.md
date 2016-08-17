+++
categories = ["Development", "Arduino"]
date = "2016-08-16T17:07:14-05:00"
description = ""
draft = true
image = "/img/about-bg.jpg"
tags = ["c/c++", "arduino", "development", "accuremote"]
title = "Reading from Accuremote Scales with Arduino"

+++

A couple of weeks ago, I started working on a project that requires me to find the position of single moving object in real time and record it. For this project, I am using a linear encoder to get this data. For those of you that do not know, a [linear encoder](https://en.wikipedia.org/wiki/Linear_encoder) is a sensor that detects it's position on a scale (think of something like a ruler).


The linear encoder I am using is an Accuremote 38-inch scale (which can be found on the companies site [here](http://accuremoteusa.com/page8.html)). Overall it seems like a pretty nice scale. It is made of steel and comes with a little gadget (called a DRO) that lets you read where the linear encoder is on the scale in both inches and centimeters. The usual application for linear encoders is milling, and this looks like it would do just fine in that environment as is.

However, I need a computer to be able to read the linear encoder, and not a human. The DRO is attached using a USB mini cable, so it looked like it should be simple enough to read from. Before starting the project though, I decided to spend some time researching the scale. After a little bit of Googling, I found a few results.

First, [this site](http://www.yuriystoys.com/2013/12/selecting-scales-for-dro.html) helped me learn that Accuremote and iGaging scales are identical in their electronics. This was useful in pointing me towards the next site.

The next place that I found was a fairly detailed breakdown of [how the DRO communicates with the linar encoder.](http://www.shumatech.com/web/21bit_protocol?page=0,0) As it turns out, it does not actually use the USB serial protocol, just the USB connecter. This means that while it looks like it could possible communicate directly with a computer, it can't, and trying to do so could damage either the linear encoder, the computer, or both. This website also revealed that the protocol was 21-bit, and that the DRO was the master and the linear encoder was the slave. The site continues to talk about how you can use the Accuremote scale with their DRO.
