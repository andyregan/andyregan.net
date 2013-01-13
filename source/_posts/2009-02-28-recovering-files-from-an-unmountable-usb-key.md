---
title: Recovering Files From an Unmountable USB Key
comments: true
layout: post
categories:
  - Linux
tags:
  - dd
  - file recovery
  - image
  - Linux
  - photorec
  - testdisk
  - thumb drive
  - ubuntu
  - usb
  - usb key
---
The other night, myself and Rory managed to recover his friend's thesis from a wonky USB thumb drive. We used [PhotoRec][1] on Ubuntu Linux.
PhotoRec comes with the TestDisk utility. You can install TestDisk via:

{% codeblock lang:bash %}
$apt-get update
$apt-get install testdisk
{% endcodeblock %}

The first thing we did was run **dmesg** in a terminal to get a bit of info on what was going on. We ran dmesg before and after we plugged 
in the drive and noted the relevant output. The device eventually settled and was given the device location sdb by the kernel, but no 
filesystem was found.
The partition was likely corrupted somehow.

TestDisk could be used to try to repair the partition on the disk, but it could also damage the data further. We decided to make an image to work with using **dd**:

**Use dd with care! Consult the man page for more info.**
{% codeblock lang:bash %}
$sudo dd if=/dev/sdb ou=./disk_image
{% endcodeblock %}

This copied the contents of the usb key block-by-block to a file called "disk_image". The was a 2G image (the total capacity) of the wonky drive.

We then used **photorec** to try to recover any files from the image.

{% codeblock lang:bash %}
$photorec ./disk_image
{% endcodeblock %}

Select the image to use.

{% img /images/posts/recovering-files-from-an-unmountable-usb-key/photorec1.png Select the image to use %}

Select the partition type.

{% img /images/posts/recovering-files-from-an-unmountable-usb-key/photorec2.png Select the partition type %}

Select the partition. 

{% img /images/posts/recovering-files-from-an-unmountable-usb-key/photorec3.png Select the partition %}

Select the filesystem type.

{% img /images/posts/recovering-files-from-an-unmountable-usb-key/photorec4.png Select the filesystem type %}

Select where to save the recovered files.

{% img /images/posts/recovering-files-from-an-unmountable-usb-key/photorec5.png Select where to save the recoverd files %}

Watch the search in progress.

{% img /images/posts/recovering-files-from-an-unmountable-usb-key/photorec6.png Search in progress %}

Et voila! PhotoRec worked its magic and we recovered the thesis, along with all the other files.

 [1]: http://www.cgsecurity.org/wiki/PhotoRec
