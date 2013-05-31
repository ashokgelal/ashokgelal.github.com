---
author: admin
comments: false
date: 2009-01-07 06:35:14+00:00
layout: post
slug: kick-ass-guide-for-installing-oracle-on-ubuntu
title: Kick ass guide for installing Oracle on Ubuntu
wordpress_id: 256
categories:
- Hardware
- Installation
- Linux
- Software
- System
- Wallpapers
tags:
- database
- Installation
- Intrepid Ibex
- oracle
- tutorial
- ubuntu
- video
---

One of the few complaints against any Linux OS - Difficult to install software which are not in the repository or in the standard .rpm/.deb format. Windows users enjoy double clicking an executable and then clicking NEXT button few times, Mac users just need to drag that .dmg file to the Application folder. Linux users feel themselves left out and find their own way playing with the Terminal, editing different system files, copying files from here to there and finally setting up some environment variables. Whew!



<!-- more -->



[ad]





After switching to Ubuntu, I tried to install Oracle but no luck until I found [this guide from Augusto Bott](http://www.pythian.com/blogs/1355/installing-oracle-11gr1-on-ubuntu-810-intrepid-ibex) . He has really written an excellent guide on installing Oracle on different versions of Ubuntu. But again with this guide also the users need to open different files and edit them manually. It isn't that much difficult but if you are like me who often needs to reinstall/upgrade Ubuntu for one reason or other, reading the whole guide and manually editing the system files is really time consuming and cumbersome. So, to save my time for future installation of Oracle database on my Ubuntu box, I wrote a couple of scripts (four scripts to be exact). Running these four scripts will install Oracle database and will give you a fresh database to start with. To facilitate visual-learners, I've also made two videos which have been embedded below. I will explain how to proceed briefly below:





1.[ Download all four scripts](http://dl.getdropbox.com/u/83257/oraInstaller.zip).and unzip them







  1. Extract Oracle database downloaded from Oracle to a folder (such as in your home folder)



  2. Open_** 2_OraInstaller.sh**_ in a text editor and change the source/destination values. The default values assume that you have extracted the Oracle installer files in ~/oracle folder and you want to install Oracle db in /opt/oracle folder.



  3. Open _**4_OraInstaller.sh**_ in a text editor and change the name of your database instance (dbSID). The default is oraIntrepid.



  4. Fire up the Terminal and make all the files executable:






_**$chmod 755 ./1_OraInstaller.sh**_





_**$chmod 755 ./2_OraInstaller.sh**_





_**$chmod 755 ./3_OraInstaller.sh**_





_**$chmod 755 ./4_OraInstaller.sh**_







  1. Make sure you have at least 3gb free space where you want to install your Oracle DB



  2. Execute: $_**./1_OraInstaller.sh**_






You need to logoff and login once







  1. Execute: $_**./2_OraInstaller.sh**_





This will install Oracle in silent mode. Please be patient as it will take some time and be very sure that YOU ARE CONNECTED TO THE INTERNET!!!







  1. Execute: $_**./3_OraInstaller.sh**_





You need to restart you computer once at this point.







  1. Execute: $./_**4****_OraInstaller.sh**_





This will install Oracle database instance in silent mode. This will take about 15 mins so be very patient.





At this point your Oracle installation on your Ubuntu box is complete.





Here are two videos showing all the above steps:





[ad]





[youtube]http://www.youtube.com/watch?v=YcMFDFVEdrM&fmt=22[/youtube] [youtube]http://www.youtube.com/watch?v=3bWer_ZWCVw&fmt=22[/youtube]



