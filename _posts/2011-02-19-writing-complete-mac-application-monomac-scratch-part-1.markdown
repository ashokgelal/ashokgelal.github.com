---
author: admin
comments: false
date: 2011-02-19 20:13:37+00:00
layout: post
slug: writing-complete-mac-application-monomac-scratch-part-1
title: Writing a complete Mac Application using MonoMac from scratch - Part 1 of N
wordpress_id: 151
categories:
- Mono
- MonoMac
tags:
- cocoa
- corewlan
- corewlanwirelessmanager
- full application
- monomac
- tutorial
---

## Hello, MonoMac





This is the first actual part of the series that I [first talked about in my previous post](http://monotropa.com/2011/02/16/writing-a-complete-mac-application-using-monomac-from-scratch/). This post is not going to be the one where we will be writing a bunch of codes or doing bunch of GUI related stuff. This is going to be sort of a 'Hello, MonoMac' post.





At this point, I assume that you have all the tools required to write a Mac Application using _MonoMac_. You need at least _XCode 3.2_, _Mono 2.8_, _MonoDevelop 2.4.2_ and the latest version of _MonoMac_. There are tons of tutorials online which can help you install these tools, so I'm not going to talk about how to install these tools. Believe me, it is a piece of cake. Let's get started!



<!-- more -->



Fire up _MonoDevelop_, and create a new MonoMac project:





### `File > New > Solution`





[caption id="attachment_43" align="aligncenter" width="300" caption="Fig. 1: Select MonoMac Project to get started"][![](http://monotropa.com/wp-content/uploads/2011/02/1-300x232.png)](http://monotropa.com/wp-content/uploads/2011/02/1.png)[/caption]





On the rightmost panel (Fig. 1), extend C# and you will see many types of built-in project templates. The one we are interested in is _MonoMac_. Click it and then select _MonoMacProject_ from the middle panel, provide the _Name_ of the solution (_CoreWLANWirelessManager_), and _Location_ where you want to save your solution. The _Solution name_ field should be filled up for you automatically. You can change the name if you want but for our purpose let's leave it with the default value. Finally, click _OK_.
You should now have a list of classes on the rightmost panel (Solution Explorer). Make sure to switch to _Default View _(Fig. 2) to be able to browse files instead of classes:





### `View > Default`









[caption id="attachment_45" align="aligncenter" width="300" caption="Fig. 2: Make sure you are in Default View"][![](http://monotropa.com/wp-content/uploads/2011/02/3-300x189.png)](http://monotropa.com/wp-content/uploads/2011/02/3.png)[/caption]





After you are in _Default View_, extend _CoreWLANWirelssManager_ node, and you will see a bunch of files there - _Main.cs_, _info.plist_, _MainMenu.xib_, _MainWindow.xib_. If you extend _MainMenu.xib_, and _MainWindow.xib_ files, you will see more classes - two classes under _MainMenu.xib_ and there classes under _MainWindow.xib_.





Don't get overwhelmed by all these files with weird names. We will talk about each files as we touch them. For now you can blindly ignore them.





Now, if you are too excited about seeing your first Mac Application, click on the _Debug_ icon on the toolbar (the gear icon with a tiny green arrow) or hit ⌘+↵. You will see a blank window entitled _Window_ (Fig. 3).





[caption id="attachment_47" align="aligncenter" width="300" caption="Fig. 3: Not so interesting Window"][![](http://monotropa.com/wp-content/uploads/2011/02/5-300x199.png)](http://monotropa.com/wp-content/uploads/2011/02/5.png)[/caption]





Nothing interesting but you can still play with the window by resizing, minimizing and maximizing the window. One thing that you should notice is that if you try to close the window by clicking the red close button,  the window gets closed (invisible), but the application itself is not terminated. You can verify this by either looking at the dock where an icon of your application is still sitting waiting and smiling at you (see Figure 4), or notice the red _Stop_ button on the _MonoDevelop_'s toolbar (next to the _Debug button_), which implies that you are still running your application. If you want to terminate the application, you can hit ⌘+Q.





[caption id="attachment_46" align="aligncenter" width="55" caption="Figure 4"][![](http://monotropa.com/wp-content/uploads/2011/02/4.png)](http://monotropa.com/wp-content/uploads/2011/02/4.png)[/caption]





The application is sill running despite closing the window because for you the window is the main application, but the application itself doesn't know that that's your main window, and you wanted to exit the application once that window, the last one, is closed. We need to tell our application that it should exit once our last window is closed. We do so by overriding a method in _AppDelegate.cs_





Extend _MainMenu.xib_ and double-click _AppDelegate.cs_. _AppDelegate _class is our delgator for the main application which handles the main application related events for us - such as deciding what to do when the last visible window is closed. We do this by overriding different methods which gets called when some events happen. Please note that this is **NOT** the place where you want to write the codes that handles your main window - such as adding GUI controls, drawing etc.





You can see that a method _FinishedLaunching()_ has already been overridden by MonoDevelop for us. This method is called when the application has just finished launching. This is sort of a port-of-entry for your application. Also, _FinishedLaunching()_ doesn't mean that your main window has been displayed on the screen - it is **NOT**! We will talk more about this in the second part.





As you can see this method initializes a _MainWindowController_ object and asks the controller to bring it's window to the front - making it visible.





## What is that controller thing?





Developers who write applications for Mac/iOS strictly follow a design pattern called [MVC](http://en.wikipedia.org/wiki/Model%E2%80%93View%E2%80%93Controller) (Model-View-Controller). This pattern helps you separate your view, controller, and model from each other so that it is easy making changes in one of them when required. This helps in [separation of concerns](http://en.wikipedia.org/wiki/Separation_of_concerns) so that each piece is dependent on other piece as little as possible. This make it possible for a group of people to work on designing GUI (view), while other group is working on the business logic and the data components (model), and the third group of people can work on writing classes that would glue these two together (controller). It is not that one group has to work on each component; this is just one of the advantages of separating your concerns using MVC. Not only each component can be developed separately but also can be tested separately.





Also, it is not that only Mac/iOS developers follow MVC; MVC is just one of the popular patterns used for writing software, so developers from other platforms also embrace this pattern. It is not platform specific - none of the patterns are. The only difference is that _XCode_, the developer tool for Mac, imposes this pattern upfront for good. Just remember that the MVC pattern is a key to becoming a successful developer - either in Mac platform or other platforms. You can still write applications without even thinking of MVC but it is gonna suck for sure (unless, of course, you are writing an application with few LOC or 'very' few classes). There are [tons of articles ](http://www.google.com/search?q=MVC)out there which describe MVC more clearly than what I've done here. You probably want to read about MVC and learn how to use it if you want to become a serious developer.





When you create a new MonoMac application, MonoDevelop cooks up your application such that it follows this MVC pattern strictly, and lays out the files accordingly. Mono team has done an excellent job by keeping this spirit of MVC intact when working with MonoDevelop, and MonoMac.





Yawn. Too much theory!. Let's do something that code monkeys love - write code. Override method _ApplicationShouldTerminateAfterLastWindowClosed() _in_ AppDelegate.cs_. Here is little handy trick to make _MonoDevelop_ override a method for us - type _override_ and hit space, _MonoDevelop_ will show a list of methods that can be overridden, select the method that you want to override and hit enter. MonoDevelop will add everything for us (except, of course, the logic). Make sure this overridden method looks like this:







As the name of the method implies, we are saying 'YES' when being asked whether to terminate once the last window is closed. You got to love the way method names are written in Objective-C - lengthy but self-documenting.





Save your solution, and run your application. This time when you click the window's close button, your application should terminate.





[caption id="attachment_52" align="aligncenter" width="300" caption="FIg. 5: This is how your final code should read like"][![](http://monotropa.com/wp-content/uploads/2011/02/MonoDevelop-300x133.png)](http://monotropa.com/wp-content/uploads/2011/02/MonoDevelop.png)[/caption]





That's it for this part. Next time we will start working on adding GUI controls to our blank window. As I said earlier, this part was sort of a 'Hello, MonoMac' session. It's going to be more fun next few sessions, as we will work with _InterfaceBuilder_ to shape up our view, and then work on connecting events to respond to buttons, checkboxes, popup buttons etc. Stay tuned!



