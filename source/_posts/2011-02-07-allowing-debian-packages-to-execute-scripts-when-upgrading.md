---
title: Allowing Debian Packages To Execute Scripts When Upgrading
description: "If you have mounted /tmp noexec, aptitude updates may fail. Here's some possible solutions."
layout: post
categories:
  - Linux
tags:
  - debian
  - Linux
---
As [David][1] came across this morning, some [Debian][2] packages try to execute scripts in **/tmp** 
during upgrade. Often **/tmp** is mounted **noexec** for security and this can cause upgrades to 
fail. Files in partitions mounted **noexec** cannot be executed.

To complete the upgrades you can temporarily mount **/tmp** with **exec**, upgrade your packages 
and then remount as per the **noexec** flag in your **/etc/fstab**.

{% codeblock lang:bash %}
$ mount -o remount,exec /tmp
$ aptitude safe-upgrade
$ mount -o remount /tmp
{% endcodeblock %}

You can also [change the default tmp directory that apt uses][3].

{% codeblock lang:bash %}
$ echo "APT::ExtractTemplates::TempDir \"/var/local/tmp\";" >> /etc/apt/apt.conf.d/50extracttemplates
$ mkdir /var/local/tmp/
{% endcodeblock %}

 [1]: http://www.ichec.ie/about_us/david_delaharpegolden
 [2]: http://www.debian.org/
 [3]: http://wiki.mediatemple.net/w/%28ve%29:Configure_apt
