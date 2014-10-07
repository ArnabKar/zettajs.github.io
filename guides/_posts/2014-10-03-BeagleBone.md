---
layout: guide
title: How to BeagleBone
description: A guide to getting your BeagleBone connected to the internet and ready to go with Zetta. 
---

See the official BeagleBone documentation while we're building this guide. 

> [http://beagleboard.org/getting-started](http://beagleboard.org/getting-started)  

{::comment}

If you can connect to your BeagleBone by visiting [http://beaglebone.local]() then everything is going ok. If you have to use it's IP address, then internet shaing is disabled.

##Share your internet connection

Turn on internet sharing for your system, and share it with the B3

//Mac instructions

//windows instructions

## Powercycle

Powercycle the BeagleBone (Eject, unplug, plug it back in)

## DH Client

Run this from your Cloud9 IDE command prompt: 

```bash
root@beaglebone:/var/lib/cloud9# dhclient usb0
```
## Test

Try visiting [http://beaglebone.local]() again. If you can connect, you're good to go. If not, there's one more step.

## One last thing

Eject and unplug the BeagleBone then **restart your computer**. Plug the board back in *after* your system is back up, then try the [http://beaglebone.local]() link again. 

## Still not working? 

  * Links
  * to outside
  * resource

{:/comment}

{::comment}

may need to use `npm cache clean` or `npm update` to install latest zetta

Pins on the BeagleBone Black come in two banks, P8 and P9. Each bank has 46 pins. Here's a pinout diagram: 

![BeagleBone Pinout](http://insigntech.files.wordpress.com/2013/09/bbb_pinouts.jpg){:.zoom}

We'll be using bank P9. Notice that that row of pins along the inner edge of the board are evenly numbered (2, 4, 6, 8 etc...), while the exterior row is odd (1, 3, 5, 7...). Only the first and last pins of each row come with printend numbers - this helps you determine which row is even and odd but also means that you just have to count pins to get to the right one. 

{:/comment}