---
title: Webcam Motion Detection in Ubuntu Linux
layout: post
categories:
  - Linux
tags:
  - Linux
  - motion
  - security
  - ubuntu
  - usb
  - webcam
---
I bought a cheap usb webcam to add to the list of devices for my fyp. The idea is to push presence updates when motion is detected. I'll be using [Motion][1] to handle motion detection.
Detailed [installation and configuration instructions][2] can be found at InfectedProject.

You can set motion to run commands when certain events happen, such as an image is saved or a movie ends. For example, you could add the following to **/etc/motion/motion.conf** so that you get an email to you're email account when a movie ends.

{% codeblock lang:bash montion config %}
# Command to be executed when a movie file (.mpg|.avi) is closed. (default: none)
# To give the filename as an argument to a command append it with %f
on_movie_end echo %f | mutt -s "[Motion]" -a %f joe@bloggs.com
{% endcodeblock %}


 [1]: http://www.lavrsen.dk/twiki/bin/view/Motion/WebHome
 [2]: http://infectedproject.com/2008/04/11/how-to-part-1-cheap-ubuntu-based-home-security/
