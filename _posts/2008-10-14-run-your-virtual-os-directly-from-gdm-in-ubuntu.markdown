---
author: admin
comments: false
date: 2008-10-14 16:12:46+00:00
layout: post
slug: run-your-virtual-os-directly-from-gdm-in-ubuntu
title: Run your virtual OS directly from GDM in Ubuntu
wordpress_id: 173
categories:
- Desktop
- Linux
- Software
- System
- Terminal
tags:
- commands
- gdm
- Linux
- script
- System
- terminal
- tool
- ubuntu
- virtualbox
- windows xp
- xorg
- xserver
---

If you regularly run a couple of OS from your VirtualBox and want to login to those OS directly from GDM session, here is a quick way to do it. For this to work you should have already set up your VirtualBox. Here we won't be talking about how to setup VirtualBox but only how to login to a virtual OS from GDM session.



<!-- more -->



![Windows XP GDM](http://www.quicktweaks.com/wp-content/uploads/2008/10/windowsgdm.png)
    Windows XP GDM





1. Create a bash script with the following contents





[ad]





`#!/bin/bash
VirtualBox -startvm **_NameOfYourVirtualOS_**`





Replace _**yourSUPassword**_ with your password, and _**NameOfYourVirtualOS**_ with the name that you have given to your virtual OS in VirtualBox.





2. Name it something like **_windowsXPGDM_** (if you want to run Windows XP), make it executable, and then copy it to /usr/bin.





`$ chmod 755 windowsXP`GDM
`$ sudo cp windowsXPGDM /usr/bin`





2. Go to **_/usr/share/xsessions_** and create a new file with the following contents:





`[Desktop Entry]
Encoding=UTF-8
Name=WindowsXP
Comment=My Virtual WindowsXP
Exec=/usr/bin/windowsXPGDM
Icon=
Type=Application `





3. Save it with a name something like _**windowsXP.desktop**_.





4. Log out and you will see a new entry WindowsXP in your GDM session. You can now directly open VirtualBox session without even logging in to your Ubuntu machine.





[ad]



