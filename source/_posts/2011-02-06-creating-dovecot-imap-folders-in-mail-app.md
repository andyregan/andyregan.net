---
comments: true
title: Creating Dovecot IMAP Folders In Mail.App
description: >
  This post explains how to create Dovecot IMAP folders and sub-folders in
  Mail.app.
layout: post
categories:
  - General Tips
---
If you want to organise your Dovecot IMAP mailboxes into folders, it may seem intuitive to
just drag the them into a new mailbox. However, you may get the error message **Target 
mailbox doesn't allow inferior mailboxes**.

The solution is to explicitly create a folder, by adding a `/` to the end of the name you 
give a new mailbox. Folders can contain other folders and mailboxes, whereas mailboxes 
can only contain mail.

For example, in Mail.app go to **Mailbox** -> **New Mailbox** . Select where you want to 
create the new folder and enter a name with a `/` at the end. If you don't include the 
`/`, a new mailbox will be created.

[{% img center /images/posts/creating-dovecot-imap-folders-in-mail-app/Screen-shot-2011-02-06-at-17.07.53.png 'Screenshot of Mail.app' %}][1]

In the image below, you can see a folder called **TEST** that contains a sub-folder 
called **TEST2** and a mailbox called **test**.  

[{% img center /images/posts/creating-dovecot-imap-folders-in-mail-app/Screen-shot-2011-02-06-at-17.08.24.png 'Screen shot of Mail.app' %}][2]

While the above example uses Mail.app, you'll also need to add a `/` to the end of 
Dovecot IMAP folder names in other clients such as Mozilla Thunderbird.

 [1]: /images/posts/creating-dovecot-imap-folders-in-mail-app/Screen-shot-2011-02-06-at-17.07.53.png
 [2]: /images/posts/creating-dovecot-imap-folders-in-mail-app/Screen-shot-2011-02-06-at-17.08.24.png
