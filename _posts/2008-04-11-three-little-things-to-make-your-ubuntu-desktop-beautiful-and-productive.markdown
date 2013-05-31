---
author: admin
comments: false
date: 2008-04-11 19:30:33+00:00
layout: post
slug: three-little-things-to-make-your-ubuntu-desktop-beautiful-and-productive
title: Three Little Things To Make Your Ubuntu Desktop Beautiful and Productive
wordpress_id: 158
categories:
- Desktop
- Software
- tutorial
tags:
- AWN
- Desktop
- dock
- GnomeDo
- Gutsy
- Hardy
- Launchy
- ubuntu
---

For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/linux/official-tutorials/759-2/).





How can you make your Ubuntu desktop look beautiful and be productive at the same time? With these three things: <!-- more -->






  [frameworkads ad="1"]






I. Avant Window Navigator: Avant Window Navigator (AWN) is a Mac like dock-bar which (as of now) sits horizontally on the bottom of your screen (development is on the way for vertical support) helping you to launch programs and locations. You can install applets to make it more productive such as checking email, reporting local weather, showing time and calendar etc. Let's get our hands dirty (to make the desktop beautiful)





[![avn](http://www.ashokgelal.com/wp-content/uploads/2008/04/avn.png)](http://www.ashokgelal.com/2008/04/three-little-things-to-make-your-ubuntu-desktop-beautiful-and-productive/avn/) Installing Avant Window Navigator:





1) Be sure your system supports Desktop Effects. I've seen people searching the internet for hours to find out - Why AWN doesn't even start? Try typing compiz in a Terminal window. What does the output say? If it says something like ...aborting and using fallback: /usr/bin/metacity then you are out of luck.





2) Get AWN. If you are using Gutsy then you need to enable Unsupported updates (gutsy-backports) in your software repository list. Go to System>Software Sources. And in the Updates tab check Unsupported updates (gutsy-backports) Close the dialog box to let Ubuntu update the repository list automatically. If you are using Hardy you don't need to do anything as AWN has now been included in universe/gnome repository.





Now you are ready to issue the following command: sudo apt-get install avant-window-navigator awn-manager





You should be now able to run AWN from Applications>Accessories menu but AWN doesn't look so much stylish and productive without the applets.





3) To install additional applets, add following two repositories to your software sources list (for both Hardy and Gutsy)





deb http://ppa.launchpad.net/reacocard-awn/ubuntu/ gutsy main deb-src http://ppa.launchpad.net/reacocard-awn/ubuntu/ gutsy main





As soon as Ubuntu finishes reloading the information of the software repositories, you will be ready to issue the following command: sudo apt-get install avant-window-navigator-bzr awn-core-applets-bzr awn-manager-bzr





4) Now to add applets to the dock bar, right click on the edge of the dock bar, click on preferences go to Applets tab and add your favorites applets.





II. GnomeDo





GonmeDo is a launcher similar to QuickSilver for Mac and Launchy for Windows which allows you to run/open the applications/location installed in your Gnome desktop with a couple of key strokes. It searches for the installed applications with a keystroke and then you can perform the general actions such as open/play/run etc. I promise you will forget about opening the run application dialog box. What I liked about GnomeDo is its ability to learn. Suppose, if you have installed Avant Window Navigator, aMSN and Amarok and you regularly use Amarok, hitting A will bring you Amarok but not other applications.





[[![gnome-do](http://www.ashokgelal.com/wp-content/uploads/2008/04/gnome-do.jpg)](http://www.ashokgelal.com/2008/04/three-little-things-to-make-your-ubuntu-desktop-beautiful-and-productive/gnome-do/) ](http://bp1.blogger.com/_8b22-pRvxWc/R_1E3PdacQI/AAAAAAAAAtw/jAGN1m9-exk/s1600-h/2282507138_a127163474.jpg) Installing GnomeDo





1) If you are using Gutsy you need to add two lines in your software sources list:






  [frameworkads ad="1"]







  deb http://ppa.launchpad.net/do-core/ubuntu gutsy main deb-src http://ppa.launchpad.net/do-core/ubuntu gutsy main






Hardy users are lazy (oops! I mean lucky)





2) Issue the following command to install GnomeDo: sudo apt-get install gnome-do





You should be now able to run GnomeDo from Applications>Accessories menu. Once it is started, you can launch it with a shortcut Super+Space





III. Beauty Tips Tweaks:





You are now ready with the 'productive' factor. For making your desktop look a little more beautiful, you need to tweak your system a bit:





1) Remove bottom pane You don't need it anymore. Remove it by right clicking on the pane and selecting Delete this panel





2) Enable top panel transparency: Enable transparency of top panel. R-click on the panel, select Properties. On the Background tab select Solid Color and set full transparency.9076321895604071





3) Enable 3D look and transparency of AWN





R-click on the edge, select Preferences: On the Task Appearance tab set: Text Color to White; Text Shadow Color to #1B3B12 and opacity to 225; Text Background color to Black and opacity to 37; Arrows Color to White and opacity to 102





On the Bar Appearance tab set: Look to 3D look; Tick Enable rounded corners; Bar angle to 45; Bar height to 48; Icon offset to 10; Main border Color to Black and opacity to 106; Internal border Color to White and opacity to 35; uncheck Show separator between launchers and tasks





On the Glass Engine tab set: Gradient First step color to #454545 and opacity to 29; Gradient Second step color to #010101 and opacity to 68; Both Highlight First step and Second step colors to White and opacity to 11





4) Download beautiful wallpapers from here [http://www.studiotwentyeight.com/wallpapers.htm](http://www.studiotwentyeight.com/wallpapers.htm)






   [frameworkads ad="2"]






For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/linux/official-tutorials/759-2/).



