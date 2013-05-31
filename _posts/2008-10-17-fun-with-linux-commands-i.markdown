---
author: admin
comments: false
date: 2008-10-17 08:13:02+00:00
layout: post
slug: fun-with-linux-commands-i
title: Fun with Linux Commands-I
wordpress_id: 213
categories:
- Linux
- TerminalFunFive
---

Happy Linux Commanding!





From now onwards I will be posting 5 Linux commands weekly and mostly targeted to Linux newbies or to those who are not much comfortable with Linux commands. This post will serve two purposes: to learn Linux commands in a fun way without putting so much load on your memory power (that's why I will post only five commands), and to realize the power of wonderful Linux commands. You might be already familiar with some of the commands and you might be hearing some of the commands for the first time; some of the commands might be very useful and some might be just for fun. This post will appear on Fridays so that you can have some 'useful' fun on weekends. If you know any Linux commands which are fun/crappy/useful/dangerous, don't forget to share with us. Just drop them in comments or shoot me an email.



<!-- more -->





  1. Want your computer speak the current time?





`$ saytime`





This command says the current system in a male voice. You can even record your own voice. Just have a look at /user/share/saytime where all the sound files are stored. If you are ambitious, you can write a shell script to set up an alarm which will say the time.







  1. Toggle between directories





`$ cd -`





If you are working in two directories, this command comes very handy. It toggles between the current directory and the last directory you were in.







  1. Repeat the last executed command





`$ !!`





When is this useful? If you are writing a shell script and want to execute a command twice. Another case where it comes handy is you forgot to add something in front of a long command such as `apt-get install foo foo1 foo2 foo3 foo4 foo5`. You need to execute _apt-get_ command as _sudo_. To execute this command again with _sudo_, issue:





`$ sudo !!`







  1. Which Linux kernel are you using?





`$ uname -a`







  1. Where are you at?





`$ pwd`





I hope this was fun and/or helpful. Try them a couple of times, then take a long breathe, and feel comfortable. You learned five Linux commands today this week. Keep coming back to become a Linux Terminal Guru!



