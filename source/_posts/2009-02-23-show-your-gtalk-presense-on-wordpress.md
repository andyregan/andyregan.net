---
title: Show Your Gtalk Presence on WordPress
author: Andy
layout: post
permalink: /show-your-gtalk-presence-on-wordpress/
categories:
  - Web Development
tags:
  - gtalk
  - php
  - presence
  - XMPP
---
I couldn't find a plugin that showed my presence in a way that I liked so I threw together the following php script. It's influenced by the [Gtalk (Status) to WordPress (G2W)][1] plugin by Aw Guo. It includes [simplehtmldom][2], a very easy to use HTML DOM parser.

The first step is to get a [Gtalk Chatback Badge][3]. Edit the badge, making sure "Show your status message" is ticked. Copy the hashcode in the url of your badge. It's the string after after `?tk`. Replace `HASHCODE` in the following code with your hashcode.

{% codeblock lang:php gtalkStatus.php %}
<?php
include './simplehtmldom/simple_html_dom.php';
// Create DOM from URL or file&lt;br />
$html = file_get_html('http://www.google.com/talk/service/badge/Show?tk=HASHCODE');
foreach($html->find('img#b') as $element){
$status['image']='&lt;img src="http://google.com/'.$element->src . '"/>'
$status['type']=$element->alt;
}
$status['text']=preg_replace("/[^a-zA-Z0-9s t]/", "", $html->find('div.r', 1)->plaintext) ;
//print_r($status);
echo $status['image']." ".$status['text'];
?>
{% endcodeblock %}

[Check it out in action][4].

Add the following to the page where you want to show your status.

{% codeblock lang:php wordpress script %}
<?php echo file_get_contents('url/to/php/script');?>`
{% endcodeblock %}

 [1]: http://www.awflasher.com/blog/ "Visit plugin homepage"
 [2]: http://simplehtmldom.sourceforge.net/ "simplehtmldom"
 [3]: http://www.google.com/talk/service/badge/New
 [4]: http://andyregan.net/gtalk_status/gtalkStatus.php "Andy's Gtalk status"
