---
comments: true
title: Quickly Reset Linux File and Directory Permissions
description: Short Linux CLI commands to reset file and/or directory permissions.
layout: post
categories:
  - General Tips
tags:
  - Linux
  - permissions
  - ubutnu
  - web development
---
## Reset File Permissions
Just a quick post so I can keep these commands handy (since I keep forgetting them). These will allow you to quickly set
different [permissions][1] for all files (or all folders) within a specific directory in Linux.

{% codeblock lang:bash reset files %}
$ find /path/to/directory/ -type f -exec chmod 644 {} \;
{% endcodeblock %}

## Reset Directory Permissions

{% codeblock lang:bash reset directories %}
$ find /path/to/directory/ -type d -exec chmod 755 {} \;
{% endcodeblock %}

## Reset Permissions on 777 PHP Files Only

{% codeblock lang:bash reset 777 php files %}
$ find /path/to/directory/ -name \*.php -perm 777 -type f -exec chmod 644 {} \;
{% endcodeblock %}


 [1]: http://www.linuxquestions.org/linux/answers/Security/Quick_and_Dirty_Guide_to_Linux_File_Permissions
