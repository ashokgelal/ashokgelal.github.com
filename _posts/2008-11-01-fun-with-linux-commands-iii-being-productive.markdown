---
author: admin
comments: false
date: 2008-11-01 07:06:44+00:00
layout: post
slug: fun-with-linux-commands-iii-being-productive
title: Fun with Linux Commands-III - Being productive
wordpress_id: 252
categories:
- Linux
- TerminalFunFive
tags:
- bash
- command
- script
- TerminalFunFive
---

Happy Linux commanding!





Who says Linux commands are just for geek people? And who says it is just a fun toy? Linux is simple yet productive, the only limitation is your imagination. Those who argue me with me for Linux being simple, here is a popular saying:





> *NIX is basically a simple operating system, but you have to be a genius to understand the simplicity



<!-- more -->



In this post, we will talk about few commands and write a couple of scripts (don't worry, it will be damn simple). Some guys might blame that these commands/ scripts have no use and might shout "why the hell do we need that." Remember, these are just the tools. It's upto you how well you use these tools for your tasks. Also remember, one who discovers the alternative uses of a tool is often called a Genious. Let's get started:





[ad]







  1. Make Linux speak that he loves himself.





`espeak "I Love Linux"`





Now you should be asking why the hell I need that? Well, what about you have a document, or a story and someone in your family is blind, or can't see nicely. You don't have enough time reading the document for him/ her. Ask him to sit in front of a computer and run this: espeak < documentName





We have more to do with _espeak_, you can even output the file to a .wav file or a .ogg file so that you can record them in a CD and mail to someone you care!





Still not impressed? What about making it to read your email, or run it in the background so that it alerts you whenever a new mail arrives in your Inbox and then reads the sender's name, and subject. Also, if you are little ambitious, you can even make it say the weather, if the weather changes drastically. I won't discuss how to make it read your mails, or weather; I'm just talking about possibility. When I get some time, I'm thinking to write a script which reads my Gmails. Just keep coming back!







  1. Making your own commands.





You have heard Linux is highly customizable. How about writing your own simple command. We will write a small script which allows you a convenient way to change the directories, actually to go back several levels up. Let's suppose you are inside _/home/yourhome/a/b/c/d/e/f/g/h/i/j_ directory. You want to change the directory (cd) to several levels up. You can easily do this with somthing like _cd ../../../../.._ But what about something as similar as up 3 which will take you 3 levels up





Fireup your favorite text editor and type the following (don't be intimated by thinking that you are programming something, I will explain this script line by line, don'w worry!):





`#!/bin/bash
LEVEL=$1
for ((i = 1; i <= LEVEL; i++))
do
CDIR=../$CDIR
done
cd $CDIR
echo "You are in: "$PWD
exec /bin/bash`





Save the file as up and issue following two commands:





`$ chmod 755 ./up`





$ sudo cp up /usr/bin





Now, from your home directory try using this command:
`
$ up 2`





Where are you at? At root directory! See how easy it was? Let's see how our little script chef made pizza for us:





`#! /bin/bash` -> you are using bash script





`LEVEL=$1` -> $1 is the first parameter passed to this script assigned to LEVEL





`for ((i = 1; i <= LEVEL; i++))` -> for some times (upto LEVEL)...
`do ` -> ...we will go round...
`CDIR=../$CDIR ` -> ...creating our path and assigning it to CDIR and...
`done` -> ...when we are done...
`cd $CDIR` -> ...we will change our directory to the path we have created above and...
`echo "You are in: "$PWD ` -> ...we will let you know where you are and finally...
`exec /bin/bash` -> ...we are done so let's get a new shell





That's was not to easy but wasn't too hard either. It is not too hard to ease your repetitive tasks with a single file and increase your productivity.





Let's make another little script...







  1. What do you usually do changing a directory? List it contents right? How about this little script?





`#!/bin/bash`





cd $1





ls





exec /bin/bash





That's it! Save it as _cdls_ or something like that and then issue this command:
`$ chmod 755 & sudo cp cdls /usr/bin`





For Ubuntu users, if you want a script that keeps track of all your "apt-get" activities by posting them to your Twitter account,[ try this little handy script](http://www.quicktweaks.com/tapt/).







  1. One more thing about _cd_. Which is the fastest and easiest cd command that take you to your home directory? **_cd /home/yourhome _**? _**cd ~**_ ? _**cd**_ itself!





`$ cd`





It takes you to your home directory







  1. Tired of typing clear to clear your screen? Press `ctrl + l`





That's it for today. Happy Halloween!





[ad]



