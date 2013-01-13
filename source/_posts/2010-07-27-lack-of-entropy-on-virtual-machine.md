---
comments: true
title: Lack of Entropy on Virtual Machine
description: The problem with entropy on virtual machines and possible solutions.
layout: post
categories:
  - Linux
---
If you are generating [GPG keys][1] on a virtual machine, you may encounter the following error.

`Not enough random bytes available. Please do some other work to give the OS a chance to collect more entropy!`

Operating systems generate randomness based on hardware input. This randomness is collected for 
use in applications which require random data (such as cryptographic applications). However, a virtual OS 
does not have the same level of access to real hardware. As a result, the usual source of randomness 
expected by the operating system is not available. The measure of randomness currently available (entropy) 
can be viewed by running 

{% codeblock lang:bash entropy in linux %}
$ cat /proc/sys/kernel/random/entropy_avail
{% endcodeblock %}

If the entropy pool drains, **/dev/random** will block until additional entropy is collected. One solution 
is to use **/dev/urandom** as a source. This will not block, but will produce lower quality randomness. 
You can use **/dev/urandom** by installing [rng-tools][2] and adding the following to **/etc/default/rng-tools**.
Save and then restart rng-tools.


{% codeblock lang:bash /etc/default/rng-tools %}
HRNGDEVICE=/dev/urandom
{% endcodeblock %}

Unfortunately, using **/dev/urandom** is not suitable for my security requirements. A better solution 
would be to use a hardware entropy generator as described in [Andy Smith's excellent post][3] . 

As a short-term work-around, I decided to generate keys on a physical host with a good quality source of 
randomness and then [import][4] them on the remote host.

 [1]: http://www.gnupg.org/
 [2]: http://packages.debian.org/lenny/rng-tools
 [3]: http://strugglers.net/~andy/blog/2010/06/06/adventures-in-entropy-part-1/
 [4]: http://www.debuntu.org/how-to-import-export-gpg-key-pair
