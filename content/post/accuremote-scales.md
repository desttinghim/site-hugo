+++
categories = ["Programming", "Electronics"]
date = "2016-08-16T17:07:14-05:00"
description = ""
draft = true
image = "/img/about-bg.jpg"
tags = ["c/c++", "arduino", "accuremote", "electronics", "programming"]
title = "Linear Encoders and Arduino"

+++

A couple of weeks ago, I started working on a project that requires me to find the position of single moving object in real time and record it. For this project, I am using a linear encoder to get this data. For those of you that do not know, a [linear encoder](https://en.wikipedia.org/wiki/Linear_encoder) is a sensor that detects it's position on a scale (think of something like a ruler).


The linear encoder I am using is an Accuremote 38-inch scale (which can be found on the companies site [here](http://accuremoteusa.com/page8.html)). Overall it seems like a pretty nice scale. It is made of steel and comes with a little gadget (called a DRO) that lets you read where the linear encoder is on the scale in both inches and centimeters. The usual application for linear encoders is milling, and this looks like it would do just fine in that environment as is.

However, I need a computer to be able to read the linear encoder, and not a human. The DRO is attached using a USB mini cable, so it looked like it should be simple enough to read from. Before starting the project though, I decided to spend some time researching the scale. After a little bit of Googling, I found a few results.

## Research

First, [this site](http://www.yuriystoys.com/2013/12/selecting-scales-for-dro.html) helped me learn that Accuremote and iGaging scales are identical in their electronics. This was useful in pointing me towards the next site.

The next place that I found was a fairly detailed breakdown of [how the DRO communicates with the linar encoder.](http://www.shumatech.com/web/21bit_protocol?page=0,0) As it turns out, it does not actually use the USB serial protocol, just the USB connecter. This means that while it looks like it could possible communicate directly with a computer, it can't, and trying to do so could damage either the linear encoder, the computer, or both. This website also revealed that the protocol was 21-bit, and that the DRO was the master and the linear encoder was the slave. The site continues to talk about how you can use the Accuremote scale with their DRO.

After that, I was led back to the first website. As it turns out, this person had already made a [guide to reading from the Accuremote scale!](http://www.yuriystoys.com/2012/01/reading-gtizzly-igaging-scales-with.html) It includes code for an Arduino, as well as the possibility to send this data over bluetooth. This was almost exactly what I needed!

With these resources in mind, I was ready to start building the Arduino system.

## Implementing

I had a few requirements in mind while I was making this system.

1. I did not want to have "operate" directly on the linear encoder (cut wires, etc.)
2. I wanted the Arduino to fit in a case

At this point, I started trying to get everything to work together. The first step was to order some parts. I had an Arduino, but I needed a USB mini breakout board, as well as a case and a project board to keep everything tidy. Once the parts arrived in the mail, I followed the guide to connect the USB mini breakout board to the proper pins on the Arduino.

At this point, I tried using the latest code from yuriystoys website to run the Arduino. I uploaded the code to an Arduino Uno, and plugged everything in. I opened up the serial monitor and saw output. Success!

...

Or not. All the readings from the scale were zero. Somehow I'd encountered a bug.

## Troubleshooting

My first thought was that there was some problem in the code. I tried to look through it, but the heavy use of registers made it difficult to read. I began looking for simpler code and found the guide I have posted above, so I tried using the code there.

This also returned all zeros. After consulting with my boss, we decided to try a different Arduino. We pulled out an Arduino Mega 2560 and modified the electronics.

Again, we tested it and were getting all zeros.

It was at this point that we started using the oscilloscope. We thought that I might have connected the breakout board up wrong. However, we discovered that was not the case. Instead, one of the solder joints wasn't even connected! It looked like it was, but a continuity test proved that it was completely disconnected.

Now that we had finally found the issue, I soldered the loose joint up and tested everything once again. This time, it worked!

Well, kind of. The results I was getting were inconsistent. I looked through the code and managed to squash a few bugs, but that didn't seem to solve everything. Eventually, I remembered reading during my research that the linear encoder had a maximum amount of reads per second. I didn't know what the numbers were, so I went back to the oscilloscope.

## Reading waveforms

The oscilloscope allowed me to work out the timings pretty well. I created a crude tool using an old USB mini cable and a USB mini breakout board that allowed us to read the signals that the DRO was creating, and the responses it was receiving from the linear encoder.

The numbers we discovered looked like this:

1. There was an interval of about 800 microseconds between each read
2. Each read consisted of 21 clock pulses
3. The clock pulses were around 110 microseconds apart
4. The linear encoder takes around 10 microseconds to switch between high and low for each number

With these numbers in mind, I could get back to programming.

## Inputting Numbers

After inputting the numbers I had discovered, the scale finally gave consistent results! With that out of the way, I could start working on the computer side program.

## Conclusion

This project was fairly straightforward, despite all the problems. In my mind, it helps to emphasize just how important a skill troubleshooting and debugging are. I could probably have had this all running in under a day if I hadn't run into the hardware bugs. But as the saying goes, live and learn!
