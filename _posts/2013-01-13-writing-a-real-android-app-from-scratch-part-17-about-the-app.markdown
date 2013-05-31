---
author: admin
comments: false
date: 2013-01-13 06:05:42+00:00
layout: post
slug: writing-a-real-android-app-from-scratch-part-17-about-the-app
title: 'Writing a Real Android App from Scratch: Part 1/9 – About the App'
wordpress_id: 417
categories:
- android
- programming
- tutorial
tags:
- android
- drawables
- emulators
- tagsnap
- tagsnap_tutorial
- tutorial
---

Welcome to part 1 of the [Writing a Real Android App from Scratch](http://www.ashokgelal.com/tag/tagsnap_tutorial/) series. If you haven't already, I ask you to [read the previous part of this series](http://www.ashokgelal.com/?p=401) first. For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/android-development/official-tutorials/writing-a-real-android-app-from-scratch-tagsnap/).





## App Introduction:





I’ve promised you to guide you all along to writing a complete Android app from scratch. As mentioned in the [first post](http://www.ashokgelal.com/?p=401), this app uses, among others, LocationManager, SQLiteDatabase, Map, and Camera etc. It allows a user to get his/ her current location, capture a picture of the location (could be a park, museum, historical landmark, or anything), add some details providing a description/ title, and selecting a category from a pre-populated list of categories. The user can either use a built-in camera, or use the Gallery app to take a picture. Later, the user will be able to see a list of all these geotagged snaps. Each item can be edited, or deleted from the list. It also has a Map view that displays markers for each of these geotagged snaps. The user can select any of these markers to see more information. These snaps will be persistent – saving all the information in a local database. We will call this app **Tagsnap**.





Tagsnap will have three tabs – **CURRENT**, **LOCATIONS**, and **MAP**, for each of the app’s main features. The app is already available on the [Google Play Store](https://play.google.com/store/apps/details?id=com.ashokgelal.tagsnap). Please download it, and play with it for a while to see how it actually works.





As simple and easy this app sounds, you will soon experience the efforts require to write it. I hope this way you will also realize the amount of efforts and time developers put in developing an app. I’m not saying you don’t but probably you will realize even more. But don’t be too scared. It might be hard but not something you cannot get done. Also, this journey is supposed to be nothing short of an adventure, so the complexity will only spice it up.





## Design/ Mockups:





As much as I love code, I’m equally a visual person and always like to start with mockups before starting to code anything that has a view. Have a look at the following mockups that give you an idea about where we will be going with our app. If you have already downloaded the [app from Play Store](https://play.google.com/store/apps/details?id=com.ashokgelal.tagsnap), you will notice that some of the looks of the app look different than these mockups. That’s not a glitch but are intentional. After all, there is a reason these are called mock-ups and not screenshots.





![](https://dl.dropbox.com/u/83257/Tagsnap_Screenshots/ss1_current_tab_mockup.png)





![](https://dl.dropbox.com/u/83257/Tagsnap_Screenshots/ss2_locations_tab_mockup.png)





![](https://dl.dropbox.com/u/83257/Tagsnap_Screenshots/ss3_add_details_mockup.png)





## Running on Emulators:





I’ve tried my best not to make any of our code depend on real hardware. When there is one, such as the camera – which should work fine on emulators anyway if you have a camera on your computer that the emulator is running under, we will check the presence of a camera, and hide the button for accessing it if there is none available. But still, there are features that we will be using that are known to not work properly on emulators such as `geocoding`, reverse geocoding, maps etc. So, I suggest you to use a real device for developing and debugging this app.





## About Android Version:





We will be targeting the latest Android API Level -- **version 17, JellyBeans 4.2** with backward compatible all the way back to **version 10, Gingerbread 2.3.3**. So, anything between Android 2.3.3 and Android 4.2 should work fine. Nevertheless, I don’t have all the resources required to do testing on different devices with different Android APIs. Let me know if there is a particular device, or an Android version that you are having problem with. We will resolve the problem together.





## Setting up the Project:





As mentioned in the first post, I will be using [IntelliJ IDEA 12](http://www.jetbrains.com/idea/download/) as my IDE but [Eclipse](http://www.eclipse.org/) should work equally well. If you can, I do advice you to give IntelliJ IDEA a try. The [Community Edition is free, and open-source](http://www.jetbrains.com/idea/download/) and in many aspects better than the Eclipse IDE. Nevertheless, I won’t be referring/ mentioning to any of the IDE feature(s) in this series. So you don’t have to worry about any IDE discrepancies. If you need help on how to set up an Android project with IntelliJ IDEA, please [refer to one of my previous tutorials](http://www.ashokgelal.com/2012/12/setting-up-intellij-idea-12-with-maven-actionbarsherlock-roboelectric-androidannotations/). You can skip the parts on `AndroidAnnotations` and `Roboelectric`. Or even better, you can start by checking out the `v0.1` tag of this project’s source code [available on GitHub](https://github.com/ashokgelal/Tagsnap). To checkout, and start building from the `v0.1` tag, use:




    
    <code>git checkout –b project-setup v0.1
    </code>





The project is an [Android Maven](http://code.google.com/p/maven-android-plugin/) IntelliJ project that comes with `ActionBarSherlock` support added. The project also comes with all the drawables required for building this app (check the `drawable`, `drawable-hdpi`, `drawable-mdpi`, and `drawable-xhdpi` folders). As I told you before, I’m not going through explaining these special folders. There is already a better (and official) [explanation about drawables available](http://developer.android.com/guide/practices/screens_support.html).





## About the Drawables:





With some inspirations from similar icons and images, I’ve designed some of these drawables myself (you’re welcome :). Some of them are from [freely available Android design resource library](http://developer.android.com/design/downloads/index.html), and some of them are free images that I’ve downloaded overtime. I don’t remember the sources and authors of each and every image. But if you are the original author of any of these images, let me know, and I will add you to the credits list.





I know your fingers are itching, and you can’t wait longer to start coding this app with your own hands but make sure you have properly setup the project. We will be adding a lot of classes, resources, and interfaces etc. to this project so get it done right now rather than struggling later. Again, don't forget to download the app from [Google Play Store](https://play.google.com/store/apps/details?id=com.ashokgelal.tagsnap).





This is it for this part. In the next part, we will start by adding tabs to our app.





You can [follow me on Twitter](https://twitter.com/ashokgelal), or [add me on Google+](https://plus.google.com/102672078908622237427/posts). For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/android-development/official-tutorials/writing-a-real-android-app-from-scratch-tagsnap/).





See you in the [next part](http://www.ashokgelal.com/?p=437).  






  [frameworkads ad="2"]




