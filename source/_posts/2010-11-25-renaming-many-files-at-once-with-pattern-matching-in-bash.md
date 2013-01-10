---
title: Renaming Many Files At Once with Pattern Matching in Bash
layout: post
categories:
  - General Tips
tags:
  - awk
  - bash
  - Linux
  - sed
---
Sometimes you may need to rename many files at once in a similar way, such as removing a 
specific string from their name. Rather than manually rename the files, you can use a method 
described at [snipplr.com][1]. One of the advantages of this method is that you can preview 
the changes you will make simply by omitting the final pipe to `/bin/bash` .

Let's say you have a bunch of files which contain patterns you would like to remove. In the
following example, you might need to remove the string **PatternOne.** and **PatternTwo.** 
from each file's name.

{% codeblock lang:bash %}
$ ls -1 /path/to/files/
PatternOne.001.PatternTwo.avi
PatternOne.002.PatternTwo.avi
PatternOne.003.PatternTwo.avi
PatternOne.004.PatternTwo.avi
{% endcodeblock %}

The first step is to use `awk` to create a serious of `mv` statements.

{% codeblock lang:bash %}
$ ls *.avi | awk '{print("mv "$1" "$1)}'
mv PatternOne.001.PatternTwo.avi PatternOne.001.PatternTwo.avi
mv PatternOne.002.PatternTwo.avi PatternOne.002.PatternTwo.avi
mv PatternOne.003.PatternTwo.avi PatternOne.003.PatternTwo.avi
mv PatternOne.004.PatternTwo.avi PatternOne.004.PatternTwo.avi
{% endcodeblock %}

Next, we pipe the `mv` statements to a `sed` command which removes **PatternOne.** from 
the `mv` statement's destination file. 

{% codeblock lang:bash %}
$ ls *.avi | awk '{print("mv "$1" "$1)}' | sed 's/PatternOne.//2'
mv PatternOne.001.PatternTwo.avi 001.PatternTwo.avi
mv PatternOne.002.PatternTwo.avi 002.PatternTwo.avi
mv PatternOne.003.PatternTwo.avi 003.PatternTwo.avi
mv PatternOne.004.PatternTwo.avi 004.PatternTwo.avi
{% endcodeblock %}

Add another `sed` statement to remove **PatternTwo.**

{% codeblock lang:bash %}
$ ls *.avi | awk '{print("mv "$1" "$1)}' | sed 's/PatternOne.//2'| sed 's/PatternTwo.//2'
mv PatternOne.001.PatternTwo.avi 001.avi
mv PatternOne.002.PatternTwo.avi 002.avi
mv PatternOne.003.PatternTwo.avi 003.avi
mv PatternOne.004.PatternTwo.avi 004.avi
{% endcodeblock %}

Finally, pipe the result to `/bin/sh` so that the `mv` statements are run. 

{% codeblock lang:bash %}
$ ls *.avi | awk '{print("mv "$1" "$1)}' | sed 's/PatternOne.//2'| sed 's/PatternTwo.//2' | /bin/sh
{% endcodeblock %}

 [1]: http://snipplr.com/view/3648/batch-file-rename-with-awk-and-sed/
