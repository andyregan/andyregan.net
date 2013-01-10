---
title: 'Libvirt + KVM : Booting From An ISO Image'
description: Booting a virtual machine from an ISO image.
layout: post
categories:
  - General Tips
  - Linux
tags:
  - kvm
  - libvirt
  - virtual machine
  - virtualization
---
{% img center http://libvirt.org/libvirtLogo.png 200 libvirt logo %}

If you want to boot your domain from a CDROM (for example, if you wish to re-install the OS on the VM),
you can edit its configuration so that it boots from an ISO image on the host.

{% codeblock lang:bash %}
host:~# virsh edit name_of_domain
{% endcodeblock %}

Within the **os** section, add a **cdrom** device. Make sure that it is placed before other boot devices, such as disks.

{% codeblock lang:bash %}
<os>
    ...
    <boot dev='cdrom'/>
    <boot dev='hd'/>
</os>
{% endcodeblock %}

Add an entry for a cdrom device (or edit an existing cdrom device) such that the **type** is *file* and the **source** 
is the path to the ISO image on the host.

{% codeblock lang:bash %}
<disk type='file' device='cdrom'>
      <source file='/path/to/install_media.iso'/>
       ...
</disk>
{% endcodeblock %}

Save your changes and then reboot the domain. It should have booted into the ISO image. You can then attach a console 
and continue the installation.

{% codeblock lang:bash %}
host:~# virt-viewer name_of_domain
{% endcodeblock %}

##Edit
I forgot (of course!) that you must undo the above changes after installation. Otherwise, it will continue to boot 
into the ISO and not your shiny new OS.
