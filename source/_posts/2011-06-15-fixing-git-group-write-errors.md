---
title: Fixing Git Group Write Errors
description: >
  When many users are committing to the same git repository, the correct group
  permissions must be set.
layout: post
categories:
  - General Tips
  - Linux
tags:
  - git
  - Linux
---
{% img center /images/posts/fixing-git-group-write-errors/200px-Git-logo.svg_.png 200 "200px Git logo" %}

## The Problem

When many users are committing to the same git repository, the correct group permissions must be 
set. For example, when a user recently tried to commit to a shared git repository, it failed 
with the following error:

{% blockquote %}
fatal: failed to write object 
{% endblockquote %}

Looking at the repository's config file on the server, the **sharedRepository** variable was not set. 

## The Fix

First, I fixed the group permissions for the repository (all the users belonged to the same group).

{% codeblock lang:bash %}
$ chmod -R g+swX /path/to/repository/
{% endcodeblock %}

Then, I set the **sharedRepository** variable to *true*.

{% codeblock lang:bash %}
$ cd /path/to/repository/
$ git repo-config core.sharedRepository true
{% endcodeblock %}

## More Info

More information on this variable can be found in the git-config man page.

{% blockquote %}
core.sharedRepository  
When group (or true), the repository is made shareable between several users in a group (making sure all the files and objects are group-writable). When all (or world or everybody), the  
repository will be readable by all users, additionally to being group-shareable. When umask (or false), git will use permissions reported by umask(2). When 0xxx, where 0xxx is an octal  
number, files in the repository will have this mode value. 0xxx will override user´s umask value, and thus, users with a safe umask (0077) can use this option. Examples: 0660 is equivalent  
to group. 0640 is a repository that is group-readable but not group-writable. See git-init(1). False by default.
{% endblockquote %}
