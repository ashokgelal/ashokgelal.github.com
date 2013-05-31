---
author: admin
comments: false
date: 2011-11-06 18:19:04+00:00
layout: post
slug: automatically-setting-terminals-window-title-to-current-path
title: Automatically Setting Terminal's Window Title to Current Path
wordpress_id: 89
categories:
- scripting
tags:
- bash
- terminal
---

While in hacking mood (read Terminal mode), I often end up using multiple terminal tabs. Switching between the tabs are a little pain as every terminal (at least, those I know of) set their title to some non-sense static text. I wanted to have the title showing the current path so that I don't have to switch between all of them to find the tab I'm looking for. After spending an hour or so, I came up with something as simple as this. Google wasn't that much helpful to find this out easily.





Append the following lines to your ~/.bashrc or ~/.bash_profile or whatever your are using for dumping your bash configs.




    
     --- >8 --- >8 --- >8 CUT HERE ... >8 --- >8 --- >8








    
    <code>settitle () </code>




    
    <code>{ </code>




    
    <code> echo -ne "\033]0;${PWD/$HOME/~}\007" </code>




    
    <code>} </code>








    
    <code>PROMPT_COMMAND=settitle</code>




    
    <code>export PROMPT_COMMAND</code>
    
    --- >8 --- >8 --- >8 ... TO HERE >8 --- >8 --- >8



