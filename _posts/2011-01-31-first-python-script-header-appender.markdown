---
author: admin
comments: false
date: 2011-01-31 07:52:41+00:00
layout: post
slug: first-python-script-header-appender
title: 'First Python Script: Header Appender'
wordpress_id: 65
categories:
- programming
tags:
- code
- coding
- Python
- script
---

I started learning Python just yesterday and I'm not only making some good progress but also just finished writing my first ever Python script that processes a bunch of *.cs files and appends a header to each files (basically a copyright notice, and a license - Apache 2.0 License in this case).





So, this turned out to be pretty good and pretty productive. I needed a similar kind of script for processing my C# files that I've written for my [NetFilterFramework](https://github.com/ashokgelal/NetFilterFramework). This framework basically allows you to filter a list based on some binary expressions criteria. You just have to create a string expression such off a public property of a class e.g. Name property of a Student class.





Writing this first ever Python Script was fun. I starting was very rough and edgy. I used IDLE at first to see how it goes and then ran a number of tests on some dummy files to see the results. All went fine with some minor problems. The big stumbling block was command line parsing. Although Python's inbuilt command line parsing (using OptParse) is super awesome, [the manual on their site](http://wiki.python.org/moin/OptParse) was not that help and too verbose. So, I needed to take the help of some external sites. So was for recursively traversing a folder to read all the files. Following sites turned out to be very helpful while writing this script:





[http://www.alexonlinux.com/pythons-optparse-for-human-beings](http://www.alexonlinux.com/pythons-optparse-for-human-beings  )





[http://code.activestate.com/recipes/499305/](http://code.activestate.com/recipes/499305/  )





[http://stefaanlippens.net/getcwd](http://stefaanlippens.net/getcwd  )





And of course stackoverflow.com is always there. Thanks to heavens for this internet thing.
<!-- more -->
Here is the my first ever Python Script in its entirety:




