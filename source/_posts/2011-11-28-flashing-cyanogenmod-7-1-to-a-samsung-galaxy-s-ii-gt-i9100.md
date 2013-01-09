---
title: Flashing Cyanogenmod 7.1 to a Samsung Galaxy S II (GT-I9100)
author: Andy
excerpt: >
  This post contains some notes from my experience of installing Cyanogenmod 7.1
  to a Samsung Galaxy S II (GT-I9100).
layout: post
permalink: /flashing-cyanogenmod-7-1-to-a-samsung-galaxy-s-ii-gt-i9100/
categories:
  - General Tips
tags:
  - android
  - cyanogenmod
  - GT-I9100
  - kies
  - samsung galaxy s ii
---
**WARNING: These notes are for reference. Doing this will void your warranty and doing this wrong could brick your phone.**

{% img center /images/posts/flashing-cyanogenmod-7-1-to-a-samsung-galaxy-s-ii-gt-i9100/cyanogen-logo.png" 200 'cyanogenmod logo' 'cyanogenmod logo' %}

## Background

Taken straight from Wikipedia, [Cyanogenmod][1] is **"an after-market replacement for the firmware of over sixty cell 
phones and Internet tablets. Based on the Android mobile computer operating system, it offers features and options not 
found in the official firmware distributed by vendors of these devices"**.

The Cyanogenmod wiki and forums are the best references for flashing your phone. I followed the instructions on the 
[Samsung Galaxy S II: Full Update Guide][2]. I won't repeat those instructions here, but I'll point out some extra steps 
I needed to take.

It's not emphasized on the wiki, but (I think) you need an external SD card installed in the phone. You can use this to 
store the Cyanogenmod and Google Apps Zip files for installation, rather than using the phones internal storage.

Google Apps (Gmail, the App Market, etc) are packaged separately due to licensing restrictions. They're an optional install,
but I followed the instructions on the wiki to install them when I installed Cyanogenmod.

## Installing the ClockworkMod Recovery

### Download Mode

To put the phone into download mode, I turned it off and then held down the following buttons:

*   volume-down
*   home
*   power

### Samsung Firmware

I initially followed the instructions for Mac OS X (my OS version was OS X 10.6.8). When I connected the phone in Download 
Mode via USB to my MacBook Pro and ran the heimdall flash command, I got the following error:

{% blockquote %}
Failed to detect compatible download-mode device.
{% endblockquote %}

There's a [closed issue][3] on the Heimdall GitHub project that explains this is caused by **"faulty secondary bootloaders 
distributed by Samsung. These bad bootloaders don't adhere to the USB specification and hence OS X is unable to communicate
with them. In order to solve this issue you need to upgrade your bootloaders to newer bootloaders that Samsung have fixed 
using Windows or Linux"**.

The solution for me was to install [Samsung Kies][4] on a Windows host and update to the phone's firmware to the latest 
official release from Samsung (I went from Android 2.3.3 to 2.3.5).

The phone was then detected by Heimdall in OS X and I was able to complete the installation of the ClockworkMod Recovery 
as per the instructions on the Cyanogenmod wiki.

## Flashing CyanogenMod

### Boot into the ClockworkMod Recovery

I turned off the phone and then held down the following buttons:

*   volume-up
*   home
*   power

### Backup

It's not a mandatory step, but I performed a back up as per the instructions in the wiki.

## End Result

After I finished installing the CyanogenMod and Google Apps Zip files, I rebooted the phone. Everything went better than expected!

 [1]: http://en.wikipedia.org/wiki/CyanogenMod
 [2]: http://wiki.cyanogenmod.com/wiki/Samsung_Galaxy_S_II:_Full_Update_Guide
 [3]: https://github.com/Benjamin-Dobell/Heimdall/issues/22
 [4]: http://en.wikipedia.org/wiki/Samsung_Kies
