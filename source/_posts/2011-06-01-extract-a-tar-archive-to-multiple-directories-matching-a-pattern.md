---
title: Extract a Tar Archive to Multiple Directories Matching A Pattern
author: Andy
excerpt: >
  A bash command chain to simultaneously extract a compressed tar archive to
  multiple directories matching a pattern.
layout: post
permalink: /extract-a-tar-archive-to-multiple-directories-matching-a-pattern/
categories:
  - General Tips
tags:
  - Linux
---
Sometimes you may need to extract the same tar archive into many directories at once.

The below command extracts **/path/to/archive.tgz** into the directories from 
**/path/to/destination/00** to **/path/to/destination/29** .

{% codeblock lang:bash %}
[me@host ~]$ ls -d /path/to/destination/[0-2][0-9]/ | xargs -I {} tar -xvzf /path/to/archive.tgz -C {}
{% endcodeblock %}
