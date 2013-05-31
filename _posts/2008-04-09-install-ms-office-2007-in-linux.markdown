---
author: admin
comments: false
date: 2008-04-09 18:10:44+00:00
layout: post
slug: install-ms-office-2007-in-linux
title: Install MS Office 2007 in Linux
wordpress_id: 156
categories:
- Installation
- tutorial
- xserver
tags:
- Linux
- MS Office 2007
- Office
- Windows
- Wine
---

For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/linux/official-tutorials/install-ms-office-2007-in-linux/).





Though there are many alternatives for MS Office, no doubt MS Office 2007 is a winner. If you want to install MS 2007 in Linux follow these steps: <!-- more -->[ad]







  1. Install _Wine_


  2. Select Applications>Wine>Configure Wine to bring the Wine Configuration dialog box. 


  3. Select _Windows Version_ as _Windows Vista_ from the _Applications Tab_


  4. Override two dll files rpcrt4.dll and msxml3.dll from Libraries tab. Override them to be Native (Windows)


  5. [Download rpcrt4.dll file. Click here and save it on your desktop.](http://www.mediafire.com/?njtut9aswdk)


  6. Open the c_drive from Wine menu


  7. Delete rpcrt4.dll and msxml3.dll files from Windows/System32 directory.


  8. Copy the downloaded rpcrt4.dll file into Windows/System32 directory


  9. Download msxml3.msi file from Microsoft download site ([here is the direct link for your convenience](http://www.microsoft.com/downloads/details.aspx?FamilyID=28494391-052b-42ff-9674-f752bdca9582&DisplayLang=en)).


  10. Install msxml3.msi file from the Terminal window by issuing msiexec /i msxml3.msi


  11. Double click on setup.exe file from your MS Office 2007 installation CD. If it doesn't work just type wine setup.exe from the Terminal window.


  12. Follow the normal installation procedures.





Watch the step-by-step video:





[youtube]http://www.youtube.com/watch?v=crW8nWoEpmw[/youtube]





UPDATE: Before you install Office 2007 don't forget about winetricks. It might help to resolve many issues. Just go to [http://www.kegel.com/wine/winetricks](http://www.kegel.com/wine/winetricks), copy the text (codes) in a file winetrick. From the terminal window type **_sh winetricks_**. Check all the msxml stuffs and click install.






  [frameworkads ad="2"]






For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/linux/official-tutorials/install-ms-office-2007-in-linux/).



