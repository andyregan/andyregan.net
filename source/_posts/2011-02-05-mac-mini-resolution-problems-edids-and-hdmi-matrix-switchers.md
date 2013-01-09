---
title: 'Mac Mini Resolution Problems &#8211; EDIDs And HDMI Matrix Switchers'
author: Andy
excerpt: "If you're sending video from a Mac Mini to multiple displays, you might need to get an adapter that can manage EDID data for you."
layout: post
permalink: /mac-mini-resolution-problems-edids-and-hdmi-matrix-switchers/
categories:
  - Apple
tags:
  - Apple
  - hdmi
  - mac mini
  - video
---
[ICHEC][1]'s offices are located on opposite sides of the country. Thanks to video conferencing, 
our Dublin and Galway-based staff are able to hold frequent virtual meetings to cut down on 
travel. When staff (or visitors) need to share content, such as a presentation, they can connect 
their laptop to the video conferencing unit (a [Polycom HDX 7000][2]). They can then select the 
content to display to the local and remote members of the conference call.

To cater for different types of video output, we use a HDMI Matrix Switcher that can take input 
from a VGA, a DVI, or a HMDI cable and output to either the TV directly, the video conference 
unit or both.

[{% img center /images/posts/mac-mini-resolution-problems-edids-and-hdmi-matrix-switchers/matrix_switcher.jpg 300 'matrix switcher' 'matrix switcher' %}][7]

We recently purchased a [Mac Mini][3] for use as a permanent conference room PC connected 
through the Matrix Switcher. Unfortunately, we found that the highest resolution on TV was only
full 1080p when the Polycom was left powered off. If the Polycom was powered on, the maximum 
resolution to the TV was much lower. Worse, the Polycom intermittently failed to receive a 
signal from the Mac Mini.

[{% img center http://upload.wikimedia.org/wikipedia/commons/thumb/6/67/2010_Mac_Mini_Ports.jpg/800px-2010_Mac_Mini_Ports.jpg 300 'Mac Mini' 'Mac Mini' %}][8]

Googling around for information, it seemed that the Mac Mini was confusing the EDID information
from the TV and Polycom unit as the data was coming through the Matrix Switcher. An [EDID 
(Extended display identification data)][4] is how a digital display, like a monitor or TV, 
tells your computer about itself. Your computer, through it's graphics card, determines supported
display settings based on the EDID data is has received from your monitor. We tried a few 
different suggested solutions without success.

In a stroke of sheer luck and brilliance, [David][5] found a [Kanex USB powered mini-DVI to HDMI 
adapter][6] in a box of spare cables. One of the features of the adapter is **"an auto EDID feature 
built-in that adapts to maximum resolution depending on users display"**. He plugged it in between 
the Mac Mini and the Matrix Switcher and, lo and behold, we had consistent 1080p resolution to 
both the TV and the Polycom!

[{%img center /images/posts/mac-mini-resolution-problems-edids-and-hdmi-matrix-switchers/adapter-300x133.jpg 300 'Kanex Adapter' 'Kanex Adapter' %}][6]

Another thing to watch out for is signal distortion when using a long-ish HDMI cable from the Mac 
Mini to the TV. The shorter the cable, the better.

You might notice that the image does not quite fit the TV at first. Try setting the TV to **PC** 
or **Dot by Dot** mode. Finally, you may also need to adjust the TV's sharpness settings. We 
found that if the sharpness was set too high, the OS X fonts would look pixelated.

Hopefully, someone might find this info useful.

 [1]: http://ichec.ie
 [2]: http://www.polycom.com/products/telepresence_video/telepresence_solutions/room_telepresence/hdx7000.html
 [3]: http://www.apple.com/macmini/
 [4]: http://en.wikipedia.org/wiki/Extended_display_identification_data
 [5]: http://www.ichec.ie/about_us/david_delaharpegolden.php
 [6]: http://www.kanexlive.com/products/iAdapt51.html
 [7]: /images/posts/mac-mini-resolution-problems-edids-and-hdmi-matrix-switchers/matrix_switcher.jpg 
 [8]: http://upload.wikimedia.org/wikipedia/commons/thumb/6/67/2010_Mac_Mini_Ports.jpg/800px-2010_Mac_Mini_Ports.jpg
