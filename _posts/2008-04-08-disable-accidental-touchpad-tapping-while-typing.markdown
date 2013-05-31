---
author: admin
comments: false
date: 2008-04-08 19:02:29+00:00
layout: post
slug: disable-accidental-touchpad-tapping-while-typing
title: Disable Accidental Touchpad Tapping While Typing
wordpress_id: 155
categories:
- touchpad
tags:
- fedora
- Linux
- mouse
- SHMConfig
- synaptics
- touchpad
- ubuntu
- xorg
---

Often when you are typing, your thumb touches the touchpad and the typing start from another position. Follow these steps to disable accidental touchpad tapping:
<!-- more -->
[ad]






    
  1. Enable SHMConfig in your xorg.conf file. Press Alt+F2. It will bring you the Run Application dialog box.

    
  2. Type following and hit enter:
**_gksu gedit /etc/X11/xorg.conf_**

    
  3. It will ask you for your super user password. Give it!

    
  4. _xorg.conf _should open in _gedit_ file editor

    
  5. Look for following lines:
_Section "InputDevice"
Identifier    "Synaptics Touchpad"...
...
EndSection_

    
  6. Add the following line just before the line EndSection
**_Option        "SHMConfig"    "true"_**

    
  7. Save the file

    
  8. Open a Terminal

    
  9. Type the following command
**_syndaemon -d -t -i 5_**
_-d _runs the command in background
_-t _disables only tapping and scrolling but not mouse movements
_-i _sets the idle time in seconds after the last key has been pressed. The default is 2 seconds. I was convenient with 5 secs. It means if I don't press any key for 5 secs, the touchpad will be enabled.





Now to run make the command as a startup command follow the following steps:






    
  1. Go to _System>Preference>Sessions_

    
  2. Click on _Add_ button.

    
  3. Fill up the following data in the dialog box that appears:
_Name: { Name of the command such as: Disable Touchpad }
Command: syndaemon -d -t -i 5
Comment: { Any comments such as: Disables touchpad's tapping functionality while typing }_

    
  4. Click on OK and Close the Sessions dialog box





Enjoy tapping less typing ;-)





[ad]



