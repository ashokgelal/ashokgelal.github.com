---
author: admin
comments: false
date: 2011-02-12 05:09:34+00:00
layout: post
slug: opening-a-new-copy-of-an-application-on-mac
title: Opening a new copy of an application on Mac
wordpress_id: 327
categories:
- Mac
- Tips&amp;Tweaks
tags:
- application
- command
- open
- terminal
---

Mac doesn't like you to open multiple copies of an application. If you try to open a new copy, it will switch to the old copy. If you want to open a new copy this is what you need to do:





Fire up the terminal and type:





$ `**open -n /Applications/<name of application>.app**`





Saw that little -n switch? That's the one which tells Mac to open a new copy.



