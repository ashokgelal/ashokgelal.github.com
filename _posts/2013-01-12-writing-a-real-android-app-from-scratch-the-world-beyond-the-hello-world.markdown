---
author: admin
comments: false
date: 2013-01-12 05:31:20+00:00
layout: post
comments: true
slug: writing-a-real-android-app-from-scratch-the-world-beyond-the-hello-world
title: Writing a Real Android App from Scratch - The World Beyond the Hello, World!
wordpress_id: 401
categories:
- android
- programming
- tutorial
tags:
- actionbarsherlock
- android
- intellij
- stickylistheaders
- tagsnap_tutorial
- tutorial
---

Developing real software is difficult, time-consuming, and ugly. It seems easy when you read the manuals and design guidelines but when you sit down and actually start writing it, it won’t take you long to realize that writing anything beyond a _"Hello, World"_ program is really difficult. The problem is not with you, or with the manuals. The problem is that everything comes in pieces, and it takes a different magnitude of efforts to join the pieces together. Manuals talk about different components, and leave assembling these components to you. The main problem is that those components neither fit together well nor are enough. You learn about [Fragments](http://developer.android.com/guide/components/fragments.html) and [Action Bar](http://developer.android.com/guide/topics/ui/actionbar.html) but in a real app you won’t be able to use them with ease because they are not backward compatible. So you'll end up using [ActionBarSherlock](http://actionbarsherlock.com/) instead. To spice up your app, you also certainly want to use some other libraries such as [StickyListHeaders](https://github.com/emilsjolander/StickyListHeaders), [ViewPagerIndicator](http://viewpagerindicator.com/), [Universal Image Loader](https://github.com/nostra13/Android-Universal-Image-Loader), and [AndroidAnnotations](http://androidannotations.org) etc. Started sensing the complexities here?

Even few years back, when the platform was relatively new with fewer components, developing apps for Android was difficult. With the introduction of new components, new and deprecated APIs such as Action Bar, Contextual Action Bar, Fragments, and ViewPager etc, it has got more complicated. The most frustrating part with app development on any platform is that there are rarely any resources that actually tell you how to compose all these nice components in a real app. And even if there are, these resources are scattered and talk only few parts of the whole app. You can find documents, and download/ clone/ fork open-source apps for studying the code. There is the source – the components and the documents, and there is the destination – the app itself. What is missing is the real path that joins the source with the destination. In this tutorial series I'll attempt to draw that path.


In next several parts of this tutorial series, you will get the real experience of developing an Android app by actually writing it from scratch to finish. It is not a bunny ‘Hello, World’ app but a real working app which uses, among others, [Fragments](http://developer.android.com/guide/components/fragments.html), [Google Maps](https://developers.google.com/maps/documentation/android/), [Tabs](http://developer.android.com/design/building-blocks/tabs.html), [Geocoder](http://developer.android.com/reference/android/location/Geocoder.html), [LocationManager](http://developer.android.com/reference/android/location/LocationManager.html), [Camera](http://developer.android.com/reference/android/hardware/Camera.html), [Gallery](http://developer.android.com/reference/android/widget/Gallery.html), [SQLiteDatabase](http://developer.android.com/reference/android/database/sqlite/SQLiteDatabase.html), as well as third-party libraries such as [Universal Image Loader](https://github.com/nostra13/Android-Universal-Image-Loader), [ActionBarSherlock](http://actionbarsherlock.com/), and [StickyListHeaders](https://github.com/emilsjolander/StickyListHeaders) etc. We will also implement our own custom Action Bar as well as make use of the [Contextual Action Bar](http://developer.android.com/design/patterns/actionbar.html#contextual).


Please remember that this is going to be a long series, and I will publish it in multiple parts. I might end up compiling all the parts as an e-book but I don’t have a plan to do that right now. The well-commented source is already [hosted on GitHub](https://github.com/ashokgelal/Tagsnap). You can fork/clone it and follow along step by step.


During the development, we will break the app features into different milestones, adding one feature at a time. Each milestone will be tagged with a version number: _v0.x.x_ . So even if you missed, or messed up something, you can always start from a new milestone.


To keep things more interesting, and for the sake of brevity, I won’t be explaining concepts that have already been explained in details in other places esp. in [the official Android documentation](http://developer.android.com/guide/components/index.html). The intention of writing this tutorial is not to get bogged down with details about a component but to see how it fits together with other components. I won’t be explaining what [Activities](http://developer.android.com/guide/components/activities.html) are, or what an [AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-intro.html) file is. Believe me, there are plenty of resources out there that define and explain the basics clearly. If you got lost during this series, follow those resources, and come back. Don't try to skip something that you are having hard time to understand otherwise you'll never learn it. All these mean that this tutorial is _NOT_ for absolute beginners. This goes without saying but I do expect you to be already familiar with Java language as well as Android development basics. I will try my best to describe new topics where appropriate and will add the links to external resources for further and advanced details.


At the end of some parts, there will also be simple assignments that you can complete. **You can completely skip** these assignments as no proceeding parts will depend on any code from these assignments. The assignments will be simple, and few and far between. As a reward, few of the lucky and hard working readers, who complete all the assignments, will get a free coupon code each for the [Commonsware Warescription](http://commonsware.com/warescription). You can use these coupon codes to download the [Commonsware Android books](http://commonsware.com/Android/) for free with all the updates for six months. The books are really good and very helpful to take your Android development skills to the next level. To be able to eligible for the coupon code, you need to make your nicely commented assignment source code publicly available. **I strongly encourage you to attempt these little assignments.**


I don’t promise you to help you out with every problem that you are going to face during this tutorial series but I will try my best to help you out esp. when you are stuck with an error. If you got stuck with errors, let me know in the comments, or create an issue, and be very clear about your problem. Use something like [http://pastie.org/](http://pastie.org/) to paste the errors and your code snippets, and share the link. Don't forget about other resources for asking help such as [StackOverflow](http://stackoverflow.com/). The thing is - **don't give up!** Let minor errors not stop you from completing this series.


Also, make sure you are already familiar with the IDE that you will be using. I will be using [IntelliJ IDEA 12](http://www.jetbrains.com/idea/), and so does [the project hosted on GitHub](https://github.com/ashokgelal/Tagsnap) for this tutorial. However, be assured that there won’t be any IDE specific code to deal with.


In the first part of this series, we will talk about the app itself, get to see and play with it, and then we will finish up by setting up the project itself. I hope you are as excited as me to write a complete Android app. If you know anyone looking for a tutorial like this, let them know. You can help spread the words by tweeting about it on Twitter, or sharing it on Google+, Facebook, or any other social network sites. The more the merrier!


You can [follow me on Twitter](https://twitter.com/ashokgelal), or [add me on Google+](https://plus.google.com/102672078908622237427/posts).

**BTW, have your heard about [LightPaper](http://www.ashokgelal.com/2013/02/light-paper-made-just-for-android/)?**


See you in the [next part](http://www.ashokgelal.com/?p=417).
