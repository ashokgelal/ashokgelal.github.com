---
author: admin
comments: false
date: 2013-01-19 18:24:43+00:00
layout: post
slug: writing-a-real-android-app-from-scratch-part-27-tabs-and-fragments
title: 'Writing a Real Android App from Scratch: Part 2/9 – Tabs and Fragments'
wordpress_id: 437
categories:
- android
- programming
- tutorial
tags:
- activities
- android
- fragments
- programming
- tagsnap_tutorial
- tutorial
---

Welcome to part 2 of the [Writing a Real Android App from Scratch](http://www.ashokgelal.com/tag/tagsnap_tutorial/) series. For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/android-development/official-tutorials/writing-a-real-android-app-from-scratch-tagsnap/).





In this part, we will be adding necessary code and resources required to add the 3 tabs - **CURRENT**, **LOCATIONS**, and **MAP**. Since we have only 3 tabs, we will stick with [Android Design Guidelines for Tabs](http://developer.android.com/design/building-blocks/tabs.html) by using Fixed Tabs.





If you have done any UI programming in other platforms, and haven’t used Tabs in Android world before, you will be tempting to add a Tab widget to the main container. But that’s not actually what we need. What we actually need is to set the navigation mode of the _ActionBar_ to tabs mode. Surprised?





You might be wondering whether you can just use the vanilla [Tab Widget](http://developer.android.com/reference/android/widget/TabWidget.html). Yes, you can. But there is a nice advantage of setting the Action Bar's navigation mode to tabs – the tab headers get merged into the ActionBar when there is enough space available, as we will see at the end of this part. If you are curious, there are altogether 3 kind of navigation modes – _Standard_, _List_, and _Tabs_. You can read more about these navigation modes [here](http://developer.android.com/reference/android/app/ActionBar.html#setNavigationMode(int)).





## Prerequisite:







  * Read previous parts of this series, [Part 0](http://www.ashokgelal.com/?p=401), and [Part1](http://www.ashokgelal.com/?p=417)


  * [Read about Android Design Guidelines for Tabs](http://developer.android.com/design/building-blocks/tabs.html).


  * [Do some readings on the Action Bar](http://developer.android.com/guide/topics/ui/actionbar.html).


  * Clone [this project from GitHub](https://github.com/ashokgelal/Tagsnap), and start with the `v0.1` tag:




    
    <code>git checkout –b tabs v0.1
    </code>






## Plan of Action:







  * Set up required resources and layout.


  * Add 3 fragments - **CURRENT**, **LOCATIONS**, and **MAP**.


  * Set the Action Bar's navigation mode to tabs to actually add a tab widget.


  * Assign and add each fragment to its corresponding tab.


  * Create, set, and handle `TabListener` callbacks for each tabs.


  * Save and restore the last selected tab position when device orientation changes.





## Adding Tabs:







  1. **Open `res/layout/main.xml` file, and delete the _TextView_ - we just need a blank container:**




    
    
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            android:orientation="vertical"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
    







  1. **Add three new string resources to `res/values/strings.xml`:**




    
    
    <resources>
        …
        <string name="current">Current</string>
        <string name="locations">Locations</string>
        <string name="map">Map</string>
    </resources>
    







  1. **Add 3 new fragments, and make each of them extend `SherlockFragment`:**




    
    // file: src/main/java/com.ashokgelal.tagsnap/CurrentFragment.java
    private class CurrentFragment extends SherlockFragment {
    }
    




    
    // file: src/main/java/com.ashokgelal.tagsnap/LocationsFragment.java
    private class LocationsFragment extends SherlockFragment {
    }
    




    
    // file: src/main/java/com.ashokgelal.tagsnap/MapFragment.java
    private class MapFragment extends SherlockFragment {
    }
    





By the end of this series, we will end up with extending other types of fragment than `SherlockFragment`, but we will worry about that later.







  1. **Modify `DefaultActivity` class to extend `SherlockFragmentActivity` instead of extending `Activity`:**




    
    // file: src/main/java/com.ashokgelal.tagsnap/DefaultActivity.java
    private class DefaultActivity extends SherlockFragmentActivity {
    }
    







  1. **Next, we will add three tabs. Add a new method - `addTabs()`:**




    
    // file: src/main/java/com.ashokgelal.tagsnap/DefaultActivity.java
    ...
        private void addTabs() {
            ActionBar bar = getSupportActionBar();
            bar.setNavigationMode(ActionBar.NAVIGATION_MODE_TABS);
    
            String currentTitle = getResources().getString(R.string.current);
            ActionBar.Tab currentTab =  bar.newTab();
            currentTab.setText(currentTitle);    
            bar.addTab(currentTab);
    
            String locationsTitle =getResources().getString(R.string.locations);
            ActionBar.Tab locationsTab =  bar.newTab();
            locationsTitle.setText(locationsTitle);
            bar.addTab(locationsTab);
    
            String mapTitle =getResources().getString(R.string.map);
            ActionBar.Tab mapTab =  bar.newTab();
            mapTab.setText(mapTitle);
            bar.addTab(mapTab);
        }
    ...
    





First, we get the supported action bar and not the regular action bar because, remember, we want to make the app backward compatible. We then set the navigation mode to `ActionBar.NAVIGATION_MODE_TABS`. Then we create 3 tabs and add them to the action bar. Let's call this method as soon as our activity gets created.







  1. **Override `onCreate()`:**




    
    // file: src/main/java/com.ashokgelal.tagsnap/DefaultActivity.java
    ...
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.main);
            addTabs();
        }
    ...
    





If you run the app now, you will get a runtime exception:




    
    Unable to start activity ComponentInfo{com.ashokgelal.tagsnap/com.ashokgelal.tagsnap.DefaultActivity}: java.lang.IllegalStateException: Action Bar Tab must have a Callback …
    





So, the Action Bar Tab is looking for someone to handle a `TabListener` callback. Let’s add a new class that implements `ActionBar.TabListener` and, thus, will be responsible for handling tab selected, unselected, and reselected events:







  1. **Add a new class `TabListener`, make it implement `ActionBar.TabListener`, and override 3 required methods:**




    
    // file: src/main/java/com.ashokgelal.tagsnap/listeners/TabListener.java
    public class TabListener implements ActionBar.TabListener {
        @Override
        public void onTabSelected(ActionBar.Tab tab, FragmentTransaction ft) {
        }
    
        @Override
        public void onTabUnselected(ActionBar.Tab tab, FragmentTransaction ft) {
        }
    
        @Override
        public void onTabReselected(ActionBar.Tab tab, FragmentTransaction ft) {
        }
    }
    





Before we start filling up these 3 methods, let’s write a constructor with 3 parameters – the parent context, a tag for each fragment, and a class name. Also, when we are at it, we will also try to find a fragment by given tag to see if it already exists. If it does, we can use it later without creating a new one.







  1. **Add a constructor to `TabListener` class:**




    
    // file: src/main/java/com.ashokgelal.tagsnap/listeners/TabListener.java
    ... 
        private final FragmentActivity mActivity; 
        private final String mTag; 
        private final Class mFragmentClass; 
        private Fragment mFragment;
    
        public TabListener(FragmentActivity activity, String tag, Class fragmentClass) {
            mActivity = activity;
            mTag = tag;
            mFragmentClass = fragmentClass;
            mFragment = activity.getSupportFragmentManager().findFragmentByTag(tag);
        }
    ...
    





Now, let’s start filling the remaining 3 methods. We won’t be handling `onTabReselected()` event. You can leave it as it is. We need to handle other 2 methods. Here is what we want to do: when a tab is selected, essentially, we need to set a fragment as the main content of the tab. At any time, we’ll have 1, and only 1, fragment active. So, instead of adding a fragment, we will replace the active one with a new one. But what if we already have a fragment created before? Well, in that case `mFragment` shouldn’t be null, and so we'll reattach if it is currently detached. If the fragment is already the active one then the `onTabSelected()` will end up being a no-op. Let’s write this code.







  1. **Modify `onTabSelected()` method to:**




    
    // file: src/main/java/com.ashokgelal.tagsnap/listeners/TabListener.java
    ... 
        @Override public void onTabSelected(ActionBar.Tab tab, FragmentTransaction ft) { 
            if (mFragment == null) {
                mFragment = SherlockFragment.instantiate(mActivity, mFragmentClass.getName());      
                // place in the default root viewgroup - android.R.id.content
                ft.replace(android.R.id.content, mFragment, mTag); 
            } else { 
                if(mFragment.isDetached()) 
                    ft.attach(mFragment); 
            }
        }
    ...
    





Notice that we are using `android.R.id.content` resource and not any of our own resources. This is important because this puts the fragment in the default root viewgroup.







  1. **Modify `onTabUnselected()` method to:**




    
    // file: src/main/java/com.ashokgelal.tagsnap/listeners/TabListener.java
    ... 
        @Override public void onTabUnselected(ActionBar.Tab tab, FragmentTransaction ft) {
            if (mFragment != null){ 
                ft.detach(mFragment);
            }
        }
    ...
    





This method is simple - if we have a fragment, we just detach it from the given fragment transaction.





This is all we need to do in `TabListener` class. The next task is to set it as callbacks listener for each of our 3 tabs. For each tab, set callback to a new instance of `TabListener`. We'll pass 3 parameters to TabListener’s constructor – _FragmentActivity_ (you can pass `this`), _a unique string tag_ (you can pass title), and _the associated fragment class itself_ (`CurrentFragment.class`, `LocationsFragment.class`, and `MapFragment.class`).







  1. **Switch to `DefaultActivity`, and modify `addTabs()` to:**




    
    // file: src/main/java/com.ashokgelal.tagsnap/DefaultActivity.java
    ...
        private void addTabs() { 
            ActionBar bar = getSupportActionBar();
            bar.setNavigationMode(ActionBar.NAVIGATION_MODE_TABS);
    
            String currentTitle = getResources().getString(R.string.current);
            ActionBar.Tab currentTab =  bar.newTab();
            currentTab.setText(currentTitle);
            currentTab.setTabListener(new TabListener(this, currentTitle, CurrentFragment.class));
            bar.addTab(currentTab);
    
            String locationsTitle = getResources().getString(R.string.locations);
            ActionBar.Tab locationsTab =  bar.newTab();
            locationsTab.setText(locationsTitle);
            locationsTab.setTabListener(new TabListener(this, locationsTitle, LocationsFragment.class));
            bar.addTab(locationsTab);
        
            String mapTitle = getResources().getString(R.string.map);
            ActionBar.Tab mapTab =  bar.newTab();
            mapTab.setText(mapTitle);
            mapTab.setTabListener(new TabListener(this, mapTitle, MapFragment.class));
            bar.addTab(mapTab); 
        }
    ...
    





Run the app now. You should see 3 tabs.





![](https://dl.dropbox.com/u/83257/Tagsnap_Screenshots/ss4_TabsPortrait.png)





## Saving the Last Selected Tab:





If you rotate your device to landscape mode, you will notice that the tab merges into the Action Bar.





![](https://dl.dropbox.com/u/83257/Tagsnap_Screenshots/ss5_TabsLandscape.png)





This is the one of the nice benefits we get for free by setting the Action Bar’s navigation mode to tabs. However, we have a small problem here. If you select any other tabs than the **CURRENT** tab and then change the orientation, the app doesn't remember the last selected tab but instead sets the **CURRENT** tab as selected. This happens because when a device configuration changes, [Android destroys the active Activity and recreates it](http://developer.android.com/reference/android/app/Activity.html#ConfigurationChanges). This is part of the [Activity Lifecycle](http://developer.android.com/guide/components/activities.html#Lifecycle). Fortunately, this is very easy to fix by just saving tab’s selected state before our activity gets trashed, and then restoring the state later when the activity gets recreated.





Before an activity (or a fragment) gets destroyed, Android calls the `onSaveInstanceState()` method, if your activity/ fragment has overriden it, passing a [Bundle](http://developer.android.com/reference/android/os/Bundle.html). We will use this bundle instance to save the selected tab index. Later, in the `onCreate()` method, we will check the passed bundle instance to see if contains the tab index. If it does, we retrieve the tab index, and set it as the selected tab’s position.







  1. **Override the `onSaveInstanceState()` method:**




    
    // file: src/main/java/com.ashokgelal.tagsnap/DefaultActivity.java
    ...
        @Override protected void onSaveInstanceState(Bundle outState){
            super.onSaveInstanceState(outState);
            int index = getSupportActionBar().getSelectedNavigationIndex();
            outState.putInt("selected_tab_index", index); 
        }
    ...
    







  1. **At the end of the `onCreate()` method, after the call to `addTabs()`, add:**




    
    // file: src/main/java/com.ashokgelal.tagsnap/DefaultActivity.java
    ...
        @Override
        public void onCreate(Bundle savedInstanceState) {
            ...
            if (savedInstanceState != null) {
                int index = savedInstanceState.getInt("selected_tab_index", 0);
                getSupportActionBar().setSelectedNavigationItem(index);
            }
        }
    ...
    





Run the app, select either **LOCATIONS** or **MAP** tab, and rotate your device. The app should remember the last selected tab position.





## Next:





In the next tutorial we'll add support for fetching current location, reverse geocode the location to get the actual human-readable address, and display properly formatted address in the **CURRENT** fragment.





You can [follow me on Twitter](https://twitter.com/ashokgelal), or [add me on Google+](https://plus.google.com/102672078908622237427/posts). For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/android-development/official-tutorials/writing-a-real-android-app-from-scratch-tagsnap/).





See you in the next part.  






  [frameworkads ad="2"]




