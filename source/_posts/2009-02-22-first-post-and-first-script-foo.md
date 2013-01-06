---
title: First Post and SMS Ping Script
author: Andy
layout: post
permalink: /first-post-and-first-script-foo/
categories:
  - General Tips
tags:
  - Compsoc
  - Linux
  - meteorsms
  - o2sms
  - Ping
  - SMS
---
Thanks for visiting! The main purpose of this blog (right now) is as an archive for useful technical bits and pieces I've come across and don't want to forget. Hopefully, you might find some of these useful, too. This is my first post and I'd like to start by sharing a handy little script-foo I learned.

Over Christmas, the Compsoc admin team reinstalled our backup server. We created a fairly large (~3.6T RAID5) /home partition which took forever to build when the server booted for the first time. Rather than keep checking into the server room I figured "Wouldn't it be handy if I could just get a text when the server comes up?".

I decided to use a great tool called <a title="o2sms" href="http://o2sms.sourceforge.net/" target="_self">o2sms</a> by <a title="mackers" href="http://www.mackers.com/" target="_self">Mackers</a> that's installed on Compsoc's webserver. It allows you to send text messages via the command line. The following script intermittently pings the backup and sends an SMS to the specified number when a ping returns successfully:

`$while ! ping -W 1 -c 1 <IPAddress> 2>&1 >/dev/null; do true; done && echo "Scorpio is UP" | meteorsms <mynumber>`

Its a simple little diddy ain't it? Run it in a screen and you can detach, logout and go about your day. Drop the **!** and it will send an SMS if a ping fails (maybe the servers down). You could also replace ping with another event, such as a certain user logging in (users | grep andrewjregan) or if you recieve a new mail.
