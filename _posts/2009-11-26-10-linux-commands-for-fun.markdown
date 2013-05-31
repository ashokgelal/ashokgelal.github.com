---
author: admin
comments: false
date: 2009-11-26 22:09:46+00:00
layout: post
slug: 10-linux-commands-for-fun
title: 10 Linux commands for fun
wordpress_id: 309
categories:
- Linux
- Terminal
tags:
- commands
- fun
- Linux
- terminal
---

Here are few Linux commands you can play with for fun. Some of these might be helpful in certain situations but I've compiled them here so that you can play with it, appreciate the power of Linux commands, or just show off your Linux skills to your friends.





<!-- more -->Let me warn you first - these commands are not for newbies, those who just started using Linux or for those who want to start with Linux terminal.





[ad]





### #1 Browse and display images in Terminal





Browse and display images in Terminal? Yes! To browse the images in the current directory:
`$ sudo zgv`
To browse the images in /home/username/pictures directory:
`$ sudo zgv _/home/username/pictures_`





Note: If you get any mouse not initialized message, just unplug your mouse, type _zgv _and plug your mouse back





### #2 Burn a CD/DVD/BluRay Disk






Llet’s add a small twist; make an ISO image of a large folder and burn them to a CD/DVD.
Crate an ISO image (myISOFile) out of a folder (or filename)
`$ mkisofs –r –o _myISOFile.ISO folderOrFilename_`
Now burn the above ISO image to a CD/DVD
`$ cdrecord --device=_cdwriter-device_ -tao -eject _myISOFile.ISO_`





### #3 Create ASCII text graphics





What about creating some ASCII graphics such as the following? You can paste it in your email as a signature to impress your friends ;-)




    
     _     _
    | |   (_)_ __  _   ___  __
    | |   | | '_ | | |  / /
    | |___| | | | | |_| |>  <
    |_____|_|_| |_|__,_/_/_





`$ figlet _Linux_`
This is displayed with the default font, to use other fonts, give a font name after switch f:
`$ figlet _quick tweaks_ –f _script_`




    
    <strong>                      _                            _
                 o       | |                          | |
     __,             __  | |    _|_          _   __,  | |   ,
    /  |  |   |  |  /    |/_)    |  |  |  |_|/  /  |  |/_) / _
    _/|_/ _/|_/|_/___/| _/   |_/ / /  |__/_/|_/| _/ /
       |
       |/    
    
    </strong><code>$ figlet <em>Quick Tweaks</em> –f <em>script</em></code>




    
      _        _)      |    __ __|                   |
     |   | |   | |  __| |  /    |      / _   _` | |  /  __|
     |   | |   | | (      <     |     /  __/ (   |   < __ 
     ___\__,_|_|___|_|_  _|  _/_/ ___|__,_|_|_____/





The fonts for _figlet _are installed in **/usr/share/figlet** directory





### #4 Run remote applications in full GUI mode






As a Computer Science student, I often need access my lab computers (which have Fedora installed) through SSH. After I submit my assignments, esp. those GUI based programming assignments, I wanted to check if everything is fine. Accessing remote computer is easy:
`$ ssh _username@example.com_`
If you want to run remote applications such as OpenOffice or Eclipse, just uncomment **`ForwardX11 yes`** in **/etc/ssh/ssh_config** file. After that if you type, eclipse, for an example, the remote application will run in full GUI mode.





### #5 Split a large file into several pieces (for easy copy)






If you have a large file of about 1 GB size and have two CDs to spare (or two thumb drives of 512 MB each), how can you carry that 1GB file?
`$ split –b_500m_ _myBigFile_ _mySmallFIles_.`
To join the smaller files to get the big files back:
`$ cat _mySmallFiles_.* > _myBigFile_`





### #6 Take screenshot of a rectangular area and save it as png file





[caption id="attachment_51" align="aligncenter" width="300" caption="Screenshot taken with import command"][![My Screenshot](http://www.quicktweaks.com/wp-content/uploads/2008/10/myscreenshot-300x144.png)](http://www.quicktweaks.com/wp-content/uploads/2008/10/myscreenshot.png)[/caption]





`$ import –frame _myScreenShot.png_`





After this command, the mouse pointer changes to a set of cross-hairs; left-click and drag the mouse across an area of the screen and release the mouse to capture the selected area.





### #7 Resize an image, put a border around it, and add a comment





`$mogrify -geometry _300x200_ -border _8x8_ -comment “_Windows Sucks_” _myScreenShot.png_`





### #8 Quickly converting a .wav file to a .mp3 file






`$ lame _myMusicFile.wav myMusicFile.mp3_`





### #9 Display a nicely formatted calendar (or doing some quick maths?)






`$ cal _1972_`





Get the factorial of 10





`$ calc _10!_`





### #10 Mirror a website to your computer for offline browsing





`$ wget -mk _http://example.com_`





[ad]



