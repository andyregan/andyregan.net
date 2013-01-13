---
title: Installing Subclipse in Ubuntu Behind a Proxy
comments: true
layout: post
categories:
  - Linux
tags:
  - eclipse
  - proxy
  - subclispe
  - subversion
  - svn
  - ubuntu
---
Took me ages to figure this out. I followed these [instructions on how to install Subclise][1]. After I added the remote site and hit "Finish" the installer just timed out. I was working behind a firewall (IT LABS!) so I figured proxy settings had something to do with my problems.

Long story short, Eclipse doesn't use your global proxy settings. In Ubuntu, the location of the settings in Eclipse is **Window->Preferences->Install & Update**.

From there, you can specify your proxy host and port and successfully install [Subclipse][2].

Subclipse is a Subversion plugin for eclipse. Thanks to Nathan for helping me find the settings!

 [1]: https://help.ubuntu.com/community/EclipseSubversion
 [2]: http://subclipse.tigris.org/
