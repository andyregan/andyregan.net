---
comments: true
title: 'Firewalling Bridged VMs Using Libvirt&#8217;s Nwfilter Driver'
description: "Using libvirt's nwfilter driver, you can create simple network filtering rules in XML to firewall your virtual machines."
layout: post
categories:
  - Linux
tags:
  - libvirt
  - Linux
  - nwfilter
  - virtualization
---
[{% img center http://libvirt.org/libvirtLogo.png 200 libvirt logo %}][2]

Recently, I needed to create a virtual machine to host an internet-facing web application. One of the 
requirements for this VM included setting up a firewall on the VM's host as well as on the guest itself 
to provide an added layer of security. 

Versions of [libvirt][2] since 8.0 include the nwfilter driver. This driver allows you to define simple
network filtering rules in XML. Libvirt can then translate your rules to iptables/ebtables. More 
information and examples are available at the [Libvirt Firewall Documentation][3] page.

It's worth noting that while I found my nwfilter rules worked on Debian Squeeze (unstable), they did 
not work on Debian Lenny. Even though I had upgraded libvirt to a more recent version from backports, 
it's likely that newer versions of dependent packages, such as iptables, are required also.

Another potential gotcha is described in the libvirt documentation:

{% blockquote %}
Finally, in terms of problems we have in deployment. The biggest problem is that if the admin does 
service iptables restart all our work gets blown away. We’ve experimented with using lokkit to 
record our custom rules in a persistent config file, but that caused different problem. Admins who 
were not using lokkit for their config found that all their own rules got blown away. So we threw 
away our lokkit code. Instead we document that if you run service iptables restart, you need to 
send SIGHUP to libvirt to make it recreate its rules. 
{% endblockquote %}

Unfortunately, there appears to be a [bug][4] in the current version of libvirt-bin on Squeeze 
(0.8.3-3) that causes the libvirt daemon to crash when you try to reload. This has been fixed 
upstream. While waiting for next Debian Squeeze release of libvirt-bin, you can use the following 
trick should iptables be restarted. 

Simply, run `$virsh nwfilter-edit your-custom-template.xml` . Add a newline to the end of your XML 
and save. Virsh will re-create your defined rules.

 [2]: http://libvirt.org/
 [3]: http://libvirt.org/firewall.html
 [4]: http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=602428
