---
author: admin
comments: false
date: 2011-02-16 19:23:18+00:00
layout: post
slug: writing-a-complete-mac-application-using-monomac-from-scratch
title: Writing a complete Mac Application using MonoMac from scratch
wordpress_id: 150
categories:
- Mono
- MonoMac
tags:
- complete application
- MacOS
- mono
- monomac
- tutorial
---

Last weekend [I finished up porting](http://www.ashokgelal.com/2011/02/corewlanwirelessmanager-porting-completed/) [CoreWLANWirelessManager from Apple's Official Reference Library](http://developer.apple.com/library/mac/#samplecode/CoreWLANWirelessManager/Introduction/Intro.html#//apple_ref/doc/uid/DTS40008921-Intro-DontLinkElementID_2) to [MonoMac](http://www.mono-project.com/MonoMac). The sample is now a [part of MonoMac's official samples](https://github.com/mono/monomac/tree/master/samples/CoreWLANWirelessManager) which contains many other useful samples contributed by different developers.





I believe samples, esp. the complete ones, are the best way to learn something. All the MonoMac's 'official' samples are really well written, and are going to be the best tool to learn MonoMac. Unfortunately, it is going to be the only one for a while as MonoMac is still in its very early age and there are no books or a complete documentation available. You probably want to browse, compile, and run them if you want to learn MonoMac and start writing applications for Mac OS.





When I was working on CoreWLANWirelessManager, I realized one important issue with 'learning-by-browsing' paradigm - it is one thing to browse the source and learn from it and a totally different world to start writing a program on your own. If you have already written a couple of programs yourself, then you can browse those samples and learn from it (or use a chunk of it in your program). But what if you have never played with MonoMac before?



<!-- more -->



Let me tell you, at first time it is intimidating - you don't know what to click, where to look for something, which methods to override, and where to call your initialization methods etc. The folder structure adds more confusions to it because it doesn't look like the structure that you have accustomed with. And then, you are suddenly in an alien world when you bring up Apple's Interface Builder. You don't know where to look for certain controls (e.g. a Label - which took me about 10 mins to find; will explain someday why), and to customize it. How do you add outlets, or add actions? Again, if you are already a Cocoa guru or at least a pupil, then this should a be piece of cake for you. Otherwise believe me you will be so frustrated the first hour that you will probably bang your head against the wall. So, I've decided to save yourself from all the frustrations (and a broken head) by guiding you through all the steps required to write a complete Mac OS application from scratch.





## About the application:





You'd better read [the official explanation](http://developer.apple.com/library/mac/#samplecode/CoreWLANWirelessManager/Introduction/Intro.html#//apple_ref/doc/uid/DTS40008921-Intro-DontLinkElementID_2) of what this application is all about. In one sentence - this is a Cocoa application that helps you manage your wireless connection, and also view the various information about your wireless connection. This is what our program will look like after we are done:





[gallery]





As you can see, the UI is really sophisticated, and that's what you should be really excited about. You will learn a heck out of this application. After sending this sample to [Miguel](http://en.wikipedia.org/wiki/Miguel_de_Icaza), this is what he wrote me back:





> Hello Ashok,
Wow, you were right.  That was quite a sophisticated sample.   TheUI does a lot of things.
Love the work!





BTW, he is really a super awesome guy who is ready to help you at any time. He responds to your emails, and he appreciates your work. He is 'down to the earth' guy. If you haven't heard about him, you probably want to stop reading this post, Google his name, learn about him, follow him on Twitter, and come back.





In future posts we will talk about the functions of each GUI controls that you can see in the above screenshots. In the mean time, I strongly suggest you to download all the tools required for us to get started - XCode 3.2, Mono 2.8, and MonoDevelop 2.4 with MonoMac addin installed. If you don't have a Mac, then you are out of luck. I also recommend you to d[ownload the sample code from Apple's website](http://developer.apple.com/library/mac/#samplecode/CoreWLANWirelessManager/Introduction/Intro.html#//apple_ref/doc/uid/DTS40008921-Intro-DontLinkElementID_2), compile, and run it to see what it does. Don't worry if you don't understand anything. Just run it to get a feel of it so that you know where we are heading. If you are too impatient, you can download/ fork/ clone the complete example from github (either from [my repo](https://github.com/ashokgelal/CoreWLANWirelessManager) or [official MonoMac repo](https://github.com/mono/monomac/tree/master/samples/CoreWLANWirelessManager)) but in that case you probably don't belong to this series of tutorials. This is going to be a step by step tutorial, so by downloading the sample and getting everything, I assume that you already know how to code in MonoMac.





This is just a teaser about the upcoming exciting few weeks where we will be writing a real-world application. Most of the tutorials you get online only show you a 'Hello, World' example and then leave you in darkness on your own. I will try my best to make this series as complete as possible. Leave your feedback, comment, or [DM me on Twitter](http://twitter.com/ashokgelal).



