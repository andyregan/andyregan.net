---
comments: true
title: How to Use SSH Tunneling to Access A Subversion Server Behind NAT
description: How to use reverse ssh tunnels to access a subversion or git repository behind NAT. 
layout: post
categories:
  - Linux
tags:
  - git
  - Linux
  - os x
  - shh tunnel
  - subversion
  - svn
---
## Now why would I want to do that?

Let's say that you have a subversion repository hosted on a PC in your office. It cannot be 
accessed directly from the outside world because it is behind NAT.

If you can't connect to a VPN or forward ports, but you **can** SSH from the server hosting 
your repository to the client that will access the repository, then your could try using a 
reverse SSH tunnel.

## Example

In the following example, I'm assuming that both the server hosting the subversion repository 
and the client trying to access the repository are running either a Linux distro or OS X. I'm 
also assuming that you are accessing the repository using svn+ssh and that openssh (or 
similar) is installed on both the client and server. (That's a lot of assumptions!)

For the sake of simplicity, I'll call the subversion server **host-a** and the client **host-b**.

### On Host-A

Since you can SSH from host-a to host-b, the first thing you'll need to do is create a reverse 
SHH tunnel. Run the following command in a terminal on host-a where **username** is a valid 
username on host-b.

{% codeblock lang:bash %}
$ ssh -fN -R 3022:localhost:22 username@host-b
{% endcodeblock %}

This will prompt you to enter your account password on host-b and will open a new reverse 
tunnel in the background. You can confirm that the tunnel is active using the ps command.

{% codeblock lang:bash %}
[host-a]$ ps aux | grep 3022:
{% endcodeblock %}

###On Host-B
Next, (from any host) log into your account on host-b . Using the following command, you 
should see that host-b is listening for incoming connections on port 3022.

{% codeblock lang:bash %}
[host-b]$netstat -aln | grep 3022
{% endcodeblock %}
        
Now edit the file at `~/.subversion/config` so that the following line is under the `[tunnels]` 
section:
        
{% codeblock lang:bash ~/.subversion/config %}
[tunnels]
sshtunnel = ssh -p 3022
{% endcodeblock %}
        
You can then check out your repository from host-a with the following command where **username** is 
your username on host-a.
**(Note that the tunnel you opened from the subversion server must be still open)**
        
{% codeblock lang:bash %}
$ svn co svn+sshtunnel://username@localhost/path/to/svn/repository /path/to/local/copy
{% endcodeblock %}
        
Once you've initially checked out the repository, you can add files, commit and update as you 
normally would (as long as the ssh tunnel is still open from host-a).
        
###Horrible Attempt At A Helpful Diagram
        
Sorry folks!

{% img center /images/posts/how-to-use-ssh-tunneling-to-access-a-subversion-server-behind-nat/tunnel_example.png "ssh tunnel example" "ssh tunnel example" %}
        
###What do I need to do if I'm using git and not subversion?
        
Glad you asked. You'll need to add the following to `~/.ssh/config` on host-b.
        
{% codeblock lang:bash ~/.ssh/config %}
Host host-a
HostName localhost
Port 3022
{% endcodeblock %}
        
Then you can just use use `[host-b]$ssh username@host-a` instead of `[host-b]$ssh -p 3022 localhost` .
        
So to clone the repository om host-a:
        
{% codeblock lang:bash %}
[host-b]git clone git+ssh://host-a/path/to/repo /path/to/local/clone
{% endcodeblock %}
        
###Resources
        
The following links contain more reverse tunnel tricks.
        
* http://www.marksanborn.net/howto/bypass-firewall-and-nat-with-reverse-ssh-tunnel/
* http://rhnotebook.wordpress.com/2010/02/13/reverse-ssh-port-forwarding-t-o-i-c-o-r-g/
