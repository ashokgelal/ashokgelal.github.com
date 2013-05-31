---
author: admin
comments: false
date: 2008-09-27 19:32:40+00:00
layout: post
slug: gmail-weather-beauty-right-on-your-ubuntu-desktop
title: Gmail+Weather+Beauty right on your Ubuntu desktop
wordpress_id: 165
categories:
- Desktop
- Linux
- tutorial
tags:
- AWN
- conky
- gnome-do
- Linux
- ubuntu
---

For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/linux/official-tutorials/gmailweatherbeauty-right-on-your-ubuntu-desktop/).





[**Update1:** I've received few complaints about weather not being displayed. Yahoo changed their API (url) such that our script was never able to grab the weather info. I've now updated the scripts, and also updated this instruction. Leave a comment to let me know whether it works or not. Thanks!]





[**Update2:** One of the readers, Areux, sent me his updated files. If you are having problem with this tutorial or just want to try out his,  [download this file](http://dl.dropbox.com/u/83257/simpleconky.tar.gz), untar it and follow the instructions in README. The following steps don't apply to his scripts. Thanks, Areux!]





Ever wished that you had new mails notifications right on your desktop? Ever wished you knew the weather info right on your desktop? Ever wished you had your hardware information right on your desktop? Ever wished your desktop was productive and beautiful at the same time? Ever wished you didn't have to use Mac4Lin theme to hide the 'ugliness' of Ubuntu's native appearance? Ever wished you had a desktop that looked as beautiful as mine? Let's fulfil your wish:





[![screenshot-1](http://www.ashokgelal.com/wp-content/uploads/2008/09/screenshot-1-300x187.png)](http://www.ashokgelal.com/2008/09/gmail-weather-beauty-right-on-your-ubuntu-desktop/screenshot-1/)










<!-- more -->



I have already talked about [how to make your Ubuntu desktop look beautiful and productive](http://www.quicktweaks.com/2008/04/11/three-little-things-to-make-your-ubuntu-desktop-beautiful-and-productive/). This time I will focus mostly on [Conky](http://conky.sourceforge.net/). Conky is a lightweight system monitor for X. Not only monitoring system, with a little bit of scripting, it can be useful to know your new mails, weather info etc.





Here I've attached all the required files, attached my Conky configuration file and explained how to make your desktop look similar to mine.






  [frameworkads ad="1"]







  **_[Updated]_**






1. [Read my previous post about AWN and Gnome-Do](http://www.quicktweaks.com/2008/04/11/three-little-things-to-make-your-ubuntu-desktop-beautiful-and-productive/).







  1. Install Conky: `     $ sudo apt-get install conky`





3. [Download this file and save in your home directory](http://dl.getdropbox.com/u/83257/.conkyrc)(Right-click, Save Link As...save the file as .conkyrc)







  1. Make a directory **_scripts_** in your home directory. [Download these scripts, extract and copy them inside the directory you just created](http://dl.getdropbox.com/u/83257/conkyscripts.zip).





5. [Download all these fonts, extract and copy them inside .fonts directory](http://dl.getdropbox.com/u/83257/conkyfonts.zip) in your home directory. If you don't have the .fonts directory, you need to create it. You might need to have administrative privileges to create this directory.







  1. Open .conkyrc file. Look for this line: **`     ${execpi 300 python ~/scripts/gmail_parser.py yourgmailusername yourgmailpassword 3}`**



  2. Replace _yourgmailusername_with your username and _yourgmailpassword_ with Gmail password. You might also need to install python-feedparser (thanks Onno and Neil for bringing this to my attention) **            $ sudo apt-get install python-feedparser**



  3. To monitor your hard disk and CPU temperature install lm-sensors and hddtemp






**     $sudo apt-get install hddtemp**





**     $sudo apt-get install lm-sensors**







  1. Now you need to know the location id for your location for weather info. Head to [http://weather.yahoo.com](http://weather.yahoo.com). Enter city or zip code for your location and click go. Copy the full url location from the address bar. The url should look something like:





**          _http://weather.yahoo.com/united-states/idaho/boise-12793940/_ for Boise, ID, USA **          _http://weather.yahoo.com/nepal/central/kathmandu-2269179/_  for Kathmandu, Nepal _          http://weather.yahoo.com/united-states/new-york/new-york-2459115/_ for New York, NY, USA





If you want Celcius, instead of default Farenheit, click on C° and then grab the url. With Celcius selected, above urls will look something like: **          _http://weather.yahoo.com/united-states/idaho/boise-12793940/?unit=c_ for Boise, ID, USA **          _http://weather.yahoo.com/nepal/central/kathmandu-2269179/?unit=c_  for Kathmandu, Nepal **          _http://weather.yahoo.com/united-states/new-york/new-york-2459115/?unit=c_ for New York, NY, USA







  1. Open **_pogodynka.sh_** file look for this line `kod=weatherURLGoesHere`. Replace _weatherURLGoesHere_ with the url you got from step 10 above



  2. Create a new empty file in your home directory. Name it ***weather. ***Leave the file as it is; do nothing with the file.



  3. Bring _Run Application_dialog box (Alt+F2) type conky to launch it






If everything goes right, your desktop should be looking much 'cooler'





More info:







  * My wallpaper, The Farm House, is from [http://interfacelift.com/wallpaper_beta/details/1545/the_farmhouse.html](http://interfacelift.com/wallpaper_beta/details/1545/the_farmhouse.html)


  * Emerald Theme is[ Scaled_Black_Mod from gnome-look.org](http://gnome-look.org/content/show.php/Scaled+Black+Mod?content=45402)


  * [More variables for conky ](http://conky.sourceforge.net/variables.html)


  * [Settings variables for conky](http://conky.sourceforge.net/config_settings.html)






  [frameworkads ad="2"]














  [![screenshot-2](http://www.ashokgelal.com/wp-content/uploads/2008/09/screenshot-2-300x187.png)](http://www.ashokgelal.com/2008/09/gmail-weather-beauty-right-on-your-ubuntu-desktop/screenshot-2/)






For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/linux/official-tutorials/gmailweatherbeauty-right-on-your-ubuntu-desktop/).



