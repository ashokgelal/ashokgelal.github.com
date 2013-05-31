---
author: admin
comments: false
date: 2008-08-26 20:20:04+00:00
layout: post
slug: analyze-and-visualize-your-gmail-ing-trends
title: Analyze and visualize your Gmail-ing trends
wordpress_id: 163
categories:
- Installation
- Linux
tags:
- gmail
- Linux
- mail
- Python
- ubuntu
---

Want to know who sends you the emails most often, or which is the year/month/day/hour you receive the emails most? With the help of a handy program called [_mail-trends_](http://code.google.com/p/mail-trends/) you can easily visualize your Gmail-ing activities. Though it is a command line program but the output is a graphically rich interactive html file. And take my words, the command to launch the program is very simple.



<!-- more -->



Requirements:







  1. The only requirement for mail-trends is Python but this tutorial is for any version of Ubuntu. With little bit of more work, it should work pretty well with any flavor of Linux, Windows, or Mac.



  2. You have got a Gmail or Google Apps mail account and have enabled IMAP. _Enable IMAP from Settings>Forwarding and POP/IMAP>Enable IMAP_






Now, fire up the terminal window and get ready for some simple commands:





Python always comes shipped with Ubuntu, so you don't need to worry about it.







  1. Install [Cheetah](http://www.cheetahtemplate.org/index.html), an open source template engine, and code generation tool. Shoot the following command to install Cheetah:





`$ sudo apt-get install python-cheetah`







  1. Either get mail-trends tar file from the terminal window...





_**$  wget http://mail-trends.googlecode.com/files/mail-trends-20080326.tar.gz**_





OR visit http://mail-trends.googlecode.com to download the stable version. Also, those of you who prefer to get your hands on the latest version can download the latest code from the Subversion by issuing: _$ svn checkout http://mail-trends.googlecode.com/svn/trunk/ mail-trends_







  1. Untar the mail-trends tar'd file by issuing:





`$ tar xfv mail-trends-20080326.tar.gz`





You will get a folder called mail-trends







  1. Get inside mail-trends directory:





`$ cd mail-trends`
5. Issue the following command to analyze your Gmail





`$  python main.py --server=imap.gmail.com --use_ssl --username=**_yourgmailusername_@**gmail.com --me=_**y****ourusername**_@gmail.com --skip_labels`





It will then ask for your Gmail password.





If everything goes right, it will create a directory out inside the mail-trends directory and creates an index.html file and three other files inside the out directory.





Fire up the _index.html_ file to open an interactive visualization of your Gmails.



