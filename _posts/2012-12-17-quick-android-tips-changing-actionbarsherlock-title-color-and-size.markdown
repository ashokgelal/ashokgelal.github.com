---
author: admin
comments: false
date: 2012-12-17 03:24:44+00:00
layout: post
slug: quick-android-tips-changing-actionbarsherlock-title-color-and-size
title: 'Quick Android Tips: Changing ActionBarSherlock Title Color and Size'
wordpress_id: 381
categories:
- android
- programming
- Tips&amp;Tweaks
tags:
- android
- programming
- tips
- tutorial
---

If you are using [ActionBarSherlock](http://actionbarsherlock.com/), and want to quickly change the color, and size of the title, you need to fiddle a bit with a style resource. Here is what you need to do:





Create a resource file **styles_mytheme.xml** under **res/values** folder and copy the following. (**Note:** If you already have your theme styles defined earlier, perhaps using the [Android Action Bar Style Generator](http://jgilfelt.github.com/android-actionbarstylegenerator/), just match the style elements based on the _parent="xxx"_, and add the missing _item_ elements.)





[frameworkads ad="1"]




    
    <?xml version="1.0" encoding="utf-8"?>
    
    <resources>
        <style name="MyTheme" parent="@style/Theme.Sherlock">
            <item name="actionBarStyle">@style/MyTheme.ActionBarStyle</item>
        </style>
    
        <style name="MyTheme.ActionBarStyle" parent="@style/Widget.Sherlock.ActionBar">
            <item name="android:titleTextStyle">@style/MyTheme.TitleTextStyle</item>
        </style>
    
        <style name="MyTheme.TitleTextStyle" parent="@style/TextAppearance.Sherlock.Widget.ActionBar.Title">
            <item name="android:textColor">#CC0000</item>
            <item name="android:textSize">20sp</item>
        </style>
    
    </resources>





Now, you need to use MyTheme instead of using a SherlockTheme. Basically, in your AndroidManifest.xml file, you need to have something like:




    
    <application
            android:label="@string/app_name"
            android:theme="@style/Theme.tagsnap">
    
        ...
        ...
        ...
    </application>



