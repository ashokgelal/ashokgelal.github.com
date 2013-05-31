---
author: admin
comments: false
date: 2008-04-14 05:10:25+00:00
layout: post
slug: customizing-your-grub-screen
title: Customizing Your GRUB Screen
wordpress_id: 159
categories:
- Installation
- Linux
- Software
- System
tags:
- GIMP
- gksu
- grub
- picture
- splash
- ubuntu
- X PixMap
---

Ever wanted to show your favorite image as your GRUB splash screen in Ubuntu something similar to other distros such as Fedora? Follow these steps:
<!-- more -->
[ad]






    
  1. Get a picture and open it with GIMP.

    
  2. Go to **_Image>Mode>Indexed..._**

    
  3. Make the maximum no. of colors to 14. To do this, enter **_14_** in the box that shows the default value of 255.

    
  4. Click Convert

    
  5. Your image might not look that good, but GRUB screen can handle only 14 colors. If you don't like the 14 color picture at all, try changing the color to grayscale (_**Image>Mode>Grayscale**_). Some pictures look great in grayscale than in 14 colors.

    
  6. Go to _**Image>Scale Image...**_

    
  7. Set the width as _**640**_ pixels and height as _**480**_ pixels. You might need to click on the Chain icon to be able to set the exact size.

    
  8. Click Scale

    
  9. You have to save the file as a X PixMap image. Go to _**File>Save As...**_ In the save dialog box Click on _**Select File Size (By Extension)**_. From the drop down menu, select X PixMap image (third option from bottom). Give a filename, Save and close GIMP.Make the archive of the image:

    
  10. R-click on the the file you have just saved and select Create Archive...

    
  11. Give a name (the default should be okay). Don't forget the save the file type as .gz. From the extension menu select .gz.

    
  12. Click CreateCopy the archive file to /boot/grub/mygrubimages

    
  13. Those who know how to copy files using Terminal window can easily do that. For those who prefer graphical way of copying files should open the Run Dialog box (Alt+F2) and type:
_**gksu nautilus /boot/grub/**_ and hit enter. If it asks for your password, give it.

    
  14. Create a new folder, name it _mygrubimages_ (you can name it anything) and copy the archive file into it.

    
  15. Open /boot/grub/menu.lst. Again in the Run dialog box, type:
_**gksu gedit /bott/grub/mnu.lst**_ and hit enter. If it asks for password again then you know what to do - restart and use Windows Vista. It never nags you for doing any administrative tasks! ;-)

    
  16. In the file that opens, add a following line:
_**splashimage=/boot/grub/mysplashimages/nameOfArchiveFile.gz**_

    
  17. Save the file, restart your Ubuntu box, and get ready to be welcomed by your favorite picture





[ad]



