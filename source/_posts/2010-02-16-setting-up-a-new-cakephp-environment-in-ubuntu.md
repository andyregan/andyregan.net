---
comments: true
title: Setting Up A New CakePHP Environment in Ubuntu
description: 
  How to get around some common stumbling blocks when creating a new CakePHP
  development environment.
layout: post
categories:
  - Web Development
tags:
  - ubuntu cakephp php linux apache vhost suphp
---
[{% img center /images/posts/setting-up-a-new-cakephp-environment-in-ubuntu/cakephp_logo_250_trans-150x150.png 150 'CakePHP Logo' 'CakePHP Logo' %}][1]

[CakePHP][1] is a useful [MVC][10] framework for PHP applications. However, there are a few common gotchas when setting up a new development 
environment. In this post, I will go through how to get around these issues when setting up a new environment on a [LAMP][11] (Linux Apache 
MySQL PHP) server, in this case server running an [Ubuntu][12] OS. Please leave a comment if you have any feedback or 
can recommend any improvements on the suggestions here.

## Grab a Copy and Get Started

The first thing you'll need to do is [get a local LAMP server up and running][13]. Then, grab 
a fresh copy of CakePHP from [cakephp.org][1]. This site also has some useful resources if you'd like to learn more about the framework.

{% codeblock lang:bash Grab the source %}
$ wget http://download.github.com/cakephp-cakephp1x-ef18ab2.tar.gz
{% endcodeblock %}

Once you have downloaded the tarball, extract it and then rename the new directory to something more useful. This is the root directory of 
the application.

{% codeblock lang:bash Extract the source %}
$ tar -xvvzf cakephp-cakephp1x-ef18ab2.tar.gz  
$ mv cakephp-cakephp1x-ef18ab2/ cake_project/
{% endcodeblock %}

## Make Tmp/ Writable

Next, you need to make sure that the **tmp/** directory is writeable. To avoid using insecure 777 permissions, I recommend installing [suPHP][2].
SuPHP allows PHP scripts to be run as their owner rather than as the default Apache user (**www-data**). As a result, you can use more secure 
file and directory permissions while still allowing the **tmp/** directory to be writeable by the application. Generally, 755 permissions 
are sufficiently secure for directories and 644 for files.

Install the correct packages, enable **suphp** and disable the **php5** module.  

{% codeblock lang:bash Prepare the Environment %}
$ sudo apt-get install libapache2-mod-suphp suphp-common 
$ sudo a2enmod suphp;sudo a2dismod php5;sudo /etc/init.d/apache2 restart
{% endcodeblock %}

## Add A Dash of Salt

With that out of the way, now you need to change the default salt value for the application. CakePHP needs a random string ("salt") which is 
used to generate secure hashes. I find the [pwgen][3] utility in Linux very useful for quickly generating a random string.

Open up **app/config/core.php**. Then, replace the **random_string** value below with the output of the **pwgen** command .


{% codeblock lang:php app/config/core.php %}
Configure::write('Security.salt', 'random_string');
{% endcodeblock %}

{% codeblock lang:bash Prepare the Environment %}
$ pwgen -y 100
{% endcodeblock %}

## Point Your Domain at the Cake Directory

You can use Apache's [userdir][4] module to test your application. This will allow you to go to **http://yourdomain.com/~username"** in your 
browser where **/home/username/public_html** is the document root. However, this often conflicts with the Apache [rewrite][5] module which 
is necessary for "friendly" URLs.

To get around this conflict on a local development environment, I recommend pointing your domain at the application's root directory. In 
this way, when you go to the domain in your browser it will show you the newly installed CakePHP application.

First, add a rule to your computer's **/etc/hosts** file so that the domain points to [localhost][6].
 
{% codeblock lang:bash /etc/hosts %}
127.0.0.1       caketest.com www.caketest.com
{% endcodeblock %}

Then, add and enable a [vhost][7] for the domain you are working on.

{% codeblock lang:bash Edit the vhost %}
$ sudo vim /etc/apache2/sites-available/caketest.com
{% endcodeblock %}

{% codeblock lang:bash /etc/apache2/sites-available/caketest.com %}
<VirtualHost *:80 >  
#Basic setup  
ServerAdmin webmaster@mydomain.com  
ServerName caketest.com  
ServerAlias www.caketest.com  
DocumentRoot /home/andrew/public_html/cake
 
ErrorLog /home/andrew/public_html/cake/error_log/error.log
 
<Directory /home/andrew/public_html/cake>  
Order Deny,Allow  
Allow from all  
# Don't show indexes for directories  
Options -Indexes  
</Directory>  
</VirtualHost>
{% endcodeblock %}

{% codeblock lang:bash restart apache %}
$ sudo a2ensite caketest.com;/etc/init.d/apache2 restart
{% endcodeblock %}

## Why is Everything Black and White?

If the new Cake installation displays without any CSS markup, make sure that the Apache rewrite module is enabled. After that, your done.

{% codeblock lang:bash Enable the rewrite module %}
$ sudo a2enmod rewrite;/etc/init.d/apache2 restart
{% endcodeblock %}

At this point, you might want to check out the [CakePHP documentation][8] which will give you more information on what to do from here.
You should also check out this article on [using Netbeans as your developemnt IDE for CakePHP projects][9].

 [1]: http://cakephp.org
 [2]: http://www.suphp.org/Home.html
 [3]: http://linux.die.net/man/1/pwgen
 [4]: http://httpd.apache.org/docs/2.1/mod/mod_userdir.html
 [5]: http://httpd.apache.org/docs/1.3/mod/mod_rewrite.html
 [6]: http://en.wikipedia.org/wiki/Localhost
 [7]: http://httpd.apache.org/docs/1.3/vhosts/
 [8]: http://book.cakephp.org/
 [9]: http://www.tiplite.com/cakephp-support-in-netbeans/
 [10]: http://hubpages.com/hub/MVC
 [11]: http://en.wikipedia.org/wiki/LAMP_%28software_bundle%29
 [12]: http://www.ubuntu.com
 [13]: http://compsoc.nuigalway.ie/wiki/how_to:apache
