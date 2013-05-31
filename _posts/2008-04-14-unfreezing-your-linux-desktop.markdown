---
author: admin
comments: false
date: 2008-04-14 22:25:05+00:00
layout: post
slug: unfreezing-your-linux-desktop
title: Unfreezing Your Linux Desktop
wordpress_id: 160
categories:
- Hardware
- Linux
- System
tags:
- firefox
- freeze
- Linux
- System
- ubuntu
- unfreeze
---

> A friend of mine writes _"...when I open some heavy sites with Firefox, my Ubuntu desktop gets freeze. I can't close the Firefox so I find no other way than to restart my system. Aren't there any other ways to unfreeze the system without restarting?"_
Yes there are!
<!-- more -->
[ad]
The most efficient way to unfreeze your system is _to be patience_. Don't try to open other applications, when your system is not responding. Don't press unnecessary keys. Go have a cup of coffee, get fresh and get back. You system might have already been unfrozen or a dialog box might be waiting for you to click on that _Force Quit_ button. (I promise you won't be greeted by BSOD!)





If you don't have much patience or your system just can't unfreeze automatically, then follow these steps:






    
  1. Hit _Caps Lock_ or _Num Lock_ keys to see if your keyboard is responding, if it isn't than you're out of luck.

    
  2. Press _**Ctrl+Alt+F2**_, you will be taken to a black&white terminal window. It will ask you for your username and password.

    
  3. Type the following command to get the PID (process ID) of the program that is not responding (in this example Firefox)
_** ps -A | grep firefox**_

    
  4. It will display you the PID of firefox. You should get at least two PIDs something similar to:
_ 5961 ?        00:00:00 firefox-2
5977 ?        00:01:12 firefox-2-bin_

    
  5. Now issue the following command to kill the non-responding firefox (its PID is 5977 in this example)
**_ kill 5977
_**_[update: If you hate working with numbers you can also kill the non-responding program by issuing_: _**killall firefox-2-bin**]_**__**

    
  6. Re-check that you have successfully killed the non-responding program:
_** ps -A | grep firefox**_

    
  7. If it doesn't show you anything, then you are done. To exit and get back to your Graphical System, press _**Ctrl+Alt+F7**_





[ad]



