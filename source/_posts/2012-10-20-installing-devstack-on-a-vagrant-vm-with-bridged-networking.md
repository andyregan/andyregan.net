---
title: Installing DevStack on A Vagrant VM With Bridged Networking
author: Andy
excerpt: 'How to get past  a networking error when installing devstack on a vagrant box configured to use bridged networking.'
layout: post
permalink: /installing-devstack-on-a-vagrant-vm-with-bridged-networking/
categories:
  - General Tips
---
Background: I'm installing [DevStack][1] on an Ubuntu 12.04 VirtualBox image I provisioned with [Vagrant][2].

I'm following [a gist on how to install DevStack][3], but I hit the following error when I ran stack.sh for the first time.

{% blockquote %}
Error: either "dev" is duplicate, or "eth0" is a garbage.
{% endblockquote %}

I specified bridged mode in my Vagrantfile, so my **HOST_IP_IFACE** is eth1 not eth0. The work-around was to export the correct 
value and re-run the script.

{% codeblock lang:bash %}
$ export HOST_IP_IFACE=eth1  
$ ./stack.sh
{% endcodeblock %}

 [1]: http://devstack.org/
 [2]: http://vagrantup.com
 [3]: https://gist.github.com/2144344
