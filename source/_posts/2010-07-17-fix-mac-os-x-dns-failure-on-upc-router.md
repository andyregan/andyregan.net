---
comments: true
title: Fix Mac OS X DNS Failure on UPC Router
Description: "Using UPC and finding DNS won't resolve since upgrading to OS X 10.6.4? This post may fix issue."
layout: post
categories:
  - Random
tags:
  - DNS
  - mac
  - os x
  - UPC
---
About a week ago, I upgraded my MacBook Pro to OS X 10.6.4. Since then, I've had trouble with the internet 
connection **only** from my house and **only** on my MBP. Sometimes the connection would work fine, but 
most of the time Firefox/Safari would only load the first couple of pages and then new pages would start to 
time out.

Since I could ping/view a site by going to the server's IP address directly, the issue appeared to be with 
the MBP resolving [DNS][1]. DNS is how your computer translates a human readable URL such as 
**andyregan.net** to a machine readable IP address.

Since the problem was **only** with my UPC connection (router is Cisco EPC2425), I thought changing DNS 
provider to [Google][2] or [OpenDNS][3] might do the trick. Alas, the new DNS servers would work for the 
first few pages and then pages started timing out again.

After poking around the (extremely limited) options on the UPC router, I noticed that there were a ton of 
firewall blocks for the 192.168.1.0/24 network over the last week. Devices connecting to the router, such 
as my MBP, are assigned IP addresses within this range. 

On closer inspection, I discovered that the router's firewall was blocking the MBP from the DNS servers 
for **LAN-side UDP Flood**. I turned off the router's firewall and restarted it. Et voila! Bob's your 
uncle.

My guess is that the firewall blocks must be due to some change in DNS behavior since the last OS X 
update. Posting here to save some poor soul the heartache. It's important to note that turning off your 
router's firewall is at your own risk.

 [1]: http://en.wikipedia.org/wiki/Domain_Name_System
 [2]: http://code.google.com/speed/public-dns/
 [3]: http://www.opendns.com/
