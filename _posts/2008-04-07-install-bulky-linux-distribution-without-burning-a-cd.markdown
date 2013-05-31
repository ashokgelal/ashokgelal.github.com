---
author: admin
comments: false
date: 2008-04-07 18:41:28+00:00
layout: post
slug: install-bulky-linux-distribution-without-burning-a-cd
title: Install Bulky Linux Distribution Without Burning a CD
wordpress_id: 154
categories:
- Installation
- Linux
- Software
tags:
- cd
- dvd
- fedora
- grub
- grub4dos
- install
- Linux
- mbr
- ntfs
- vmlinuz
---

This tutorial is about installing Fedora Linux without burning a CD/DVD.





Scenario/ Assumption:
1) You have Microsoft Windows
2) You have your system drive as C: (No matter whether it is FAT or NTFS)
3) You have Fedora .ISO files in the root directory of some partition (the partition must be FAT!)
4) You need to have a program that can open up the .ISO files. You can use free file archiver. 7zip works great. In this tutorial I am using Winrar.
<!-- more -->
[ad]





Steps:






    
  1. Go to C: drive and create a directory; name it boot

    
  2. Open the .ISO file with Winrar. Inside isolinux directory, you will find two files: initrd.img and vmlinuz. Extract these two files to boot directory you have created in step 1 above

    
  3. Go to http://sourceforge.net/projects/grub4dos to download grub4dos. This is required to boot grub from dos.

    
  4. Extract the contents of grub4dos. Rename the directory to grub and copy it inside boot directory.

    
  5. Inside the grub directory, you will find two files; grldr and grldr.mbr. Copy these two files to your C: root directory

    
  6. Open boot.ini file which contains the boot information for Windows. You need to enable Show Hidden Files and Show System Files to view it in C: root directory. The easiest way is to type C:boot.ini in the Run dialog box.

    
  7. Type **_C:grldr="Start GRUB"_** at the bottom of boot.ini and save it.

    
  8. Open menu.lst file with Notepad which is inside C:bootgrub directory and type the following lines at the bottom:
**_ title Install Fedora Linux
kernel (hd0,0)/boot/vmlinuz
initrd (hd0,0)/boot/initrd.img_**

    
  9. Save the menu.lst file and restart your computer.





[ad]



