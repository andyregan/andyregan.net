---
title: Nasty Site Hack Leads to Phony Google Links
layout: post
categories:
  - Web Development
tags:
  - security
  - web development
---

##Thar be nasty scripts here!

{% img center http://andyregan.net/blog/wp-content/uploads/2009/07/tornado_warning.gif 300 'Warning Sign' 'Thar be nasty scripts here!' %}

Came across a dirty (but interesting) website hack today. Essentially when a person googled their domain name they discovered hundreds 
and hundreds of phony links pointing to a sub-directory of their site. (With various explicitly charming descriptions). 
Clicking on these links displayed a page full of advertisements.

Had a nose around the sub-directory in question and couldn't find any files corresponding to the links.

Eventually, I found a rogue PHP script (called **report.php**). Turns out that the [.htaccess][1] file in the directory had been modified 
to redirect any **[404 Not Found][2]** errors to the PHP script. Even dirtier still was that when you accessed the script directly in a 
browser it showed a typical default 404 error, a tactic I guess is there to add confusion when you are trying to find the cause of the 
hack. **Bastards**.

Script was [base64][3]'d and full of typical randomness meant to slow down those trying to figure out exactly what it's trying to do. 
As far as I can tell, it just serves up ad pages when it's redirected to and the 404 error page when accessed directly.

The ultimate bastardly thing about this hack is that even after you clean up your site, it'll be ages before the explicit phony links 
dissappear from Google. Another good reason to avoid 777 permissions of directories and keep your web software updated.

Just wanted to make people aware of it. If you come across something similar have a look in the **.htaccess** file first. :) 

##UPDATE  
The following bash one-liner can help find any files that contain a call to the base64_encode() method. Some legitimate files may be 
included, but this should help narrow down the list of dirty scripts.


{% codeblock lang:bash snippet %}
$find . -type f | xargs grep -l base64_encode
{% endcodeblock %}

Also worth checking out the following links for htaccess tips and tricks:

*   <http://www.askapache.com/htaccess/apache-htaccess.html>
*   <http://perishablepress.com/press/2006/01/10/stupid-htaccess-tricks/#sec1>

 [1]: http://httpd.apache.org/docs/1.3/howto/htaccess.html
 [2]: http://en.wikipedia.org/wiki/HTTP_404
 [3]: http://www.wewatchyourwebsite.com/wordpress/?p=136
