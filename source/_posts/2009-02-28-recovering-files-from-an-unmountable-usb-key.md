---
title: Recovering Files From an Unmountable USB Key
author: Andy
layout: post
permalink: /recovering-files-from-an-unmountable-usb-key/
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

{% img http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec1.png 150 150 [Select the image to use [screengrab of photorec]] %}

<div id='gallery-1' class='gallery galleryid-71 gallery-columns-2 gallery-size-thumbnail'>
  <dl class='gallery-item'>
    <dt class='gallery-icon'>
      <a href='http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec1.png' title='photorec1' rel="lightbox[71]"><img width="150" height="150" src="http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec1-150x150.png" class="attachment-thumbnail" alt="1. Select the image to use" title="photorec1" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption'>
      1. Select the image to use
    </dd>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon'>
      <a href='http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec2.png' title='photorec2' rel="lightbox[71]"><img width="150" height="150" src="http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec2-150x150.png" class="attachment-thumbnail" alt="2. Select the partition table type" title="photorec2" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption'>
      2. Select the partition table type
    </dd>
  </dl>
  
  <br style="clear: both" /><dl class='gallery-item'>
    <dt class='gallery-icon'>
      <a href='http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec3.png' title='photorec3' rel="lightbox[71]"><img width="150" height="150" src="http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec3-150x150.png" class="attachment-thumbnail" alt="3. Select the partition" title="photorec3" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption'>
      3. Select the partition
    </dd>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon'>
      <a href='http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec4.png' title='photorec4' rel="lightbox[71]"><img width="150" height="150" src="http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec4-150x150.png" class="attachment-thumbnail" alt="4. Select the filesystem type" title="photorec4" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption'>
      4. Select the filesystem type
    </dd>
  </dl>
  
  <br style="clear: both" /><dl class='gallery-item'>
    <dt class='gallery-icon'>
      <a href='http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec5.png' title='photorec5' rel="lightbox[71]"><img width="150" height="150" src="http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec5-150x150.png" class="attachment-thumbnail" alt="5. Select where to save recovered files" title="photorec5" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption'>
      5. Select where to save recovered files
    </dd>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon'>
      <a href='http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec6.png' title='photorec6' rel="lightbox[71]"><img width="150" height="150" src="http://andyregan.net/wordpress/wp-content/uploads/2009/02/photorec6-150x150.png" class="attachment-thumbnail" alt="6. Search in progress" title="photorec6" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption'>
      6. Search in progress
    </dd>
  </dl>
  
  <br style="clear: both" /> <br style='clear: both;' />
</div>

Et voila! PhotoRec worked its magic and we recovered the thesis, along with all the other files.

 [1]: http://www.cgsecurity.org/wiki/PhotoRec
