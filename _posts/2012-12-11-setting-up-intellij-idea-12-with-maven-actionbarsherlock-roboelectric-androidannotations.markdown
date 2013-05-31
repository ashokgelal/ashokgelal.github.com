---
author: admin
comments: false
date: 2012-12-11 08:11:30+00:00
layout: post
slug: setting-up-intellij-idea-12-with-maven-actionbarsherlock-roboelectric-androidannotations
title: Setting up IntelliJ IDEA 12 with Maven + ActionBarSherlock + Roboelectric +
  AndroidAnnotations
wordpress_id: 111
categories:
- android
- programming
- tutorial
tags:
- actionbarsherlock
- android
- androidannotations
- eclipse
- intellij
- maven
- roboelectric
---

For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/android-development/official-tutorials/setting-up-intellij-idea-12-with-maven-actionbarsherlock-roboelectric-androidannotations/).





Over the weekend I spent almost a whole day trying to setup Android development environment with recently released [IntelliJ IDEA 12](http://www.jetbrains.com/idea/). I’ve used previous versions of IntelliJ, on and off but at work we almost exclusively use Eclipse. I’ve tried using Maven before but I thought it was more pain than it was worth, and I almost never got it working perfectly. But almost all the Android/ Java developers that I know personally and follow on Social Networks look to use [Maven](http://maven.apache.org/). So, this time I was all into making it working no matter what. Honestly, I almost gave up, again.





Also, this is the first time I wanted to take [AndroidAnnotations](http://androidannotations.org/) and [Roboelectric](http://pivotal.github.com/robolectric/) for a spin. I’ve used [ActionBarSherlock](http://actionbarsherlock.com/) in all of the Android apps I’ve ever worked with but only in Eclipse.





Let’s see how can start using IntelliJ IDEA 12 for Android development along with AndroidAnnotations, Roboelectric, ActionBarSherlock, and Maven.





 





## Adding ActionBarSherlock:





1. The first thing that I’ve noticed with Android Maven integration is that the available hosted Maven Android libraries release cycle lag a bit behind than the official release. For an example, Android v4.2 is already out but the latest Android library available in Maven repo is just v4.1.2.





So, the first thing you want to is to arrange to use the libraries directly from your local Android SDK installation. This sounds difficult but it is crazy easy. Read and follow the instructions from [maven-android-sdk-deployer GitHub repo](https://github.com/mosabua/maven-android-sdk-deployer) to deploy the libraries. Don’t try to install everything in one shot (using `mvn install`). You are almost certain to get some errors; unless you have installed all the official platforms and add-on apis.





2. [Download and install IntelliJ IDEA 12](http://www.jetbrains.com/idea/download/index.html).





3.  [Download ActionBarSherlock library](http://actionbarsherlock.com/) and unzip it somewhere. You want to keep this library around, probably for multiple projects. So, keep it in a safe, non-temporary place.





4.  Inside the _ActionBarSherlock-X.X.X_ folder you will see a folder named _library_. Rename it to _ActionBarSherlock_.





5.  Fire up _IntelliJ_, and from the startup screen select **Import Project**.





[![](http://www.ashokgelal.com/wp-content/uploads/2012/12/import_screen-300x199.png)](http://www.ashokgelal.com/wp-content/uploads/2012/12/import_screen.png)





6. Browse to the unzipped directory from step 5, and select the _ActionBarSherlock_ folder (the one we renamed from _library_ in step 4).





7. Select **Create project from existing sources** radio button and click **Next**.





8. Keep clicking **Next** until you see a **Finish** button. Click it to finish the import.





9. Select **View>Tool Windows>Maven Projects** to bring up the Maven Window. The _ActionBarSherlock_ library should be listed along with _Profiles_.





10. Expand **ActionBarSherlock>Lifecycle**, and select **compile**. Click the green run button to build it.





[![](http://www.ashokgelal.com/wp-content/uploads/2012/12/lifecycle_compile-300x269.png)](http://www.ashokgelal.com/wp-content/uploads/2012/12/lifecycle_compile.png)





At this point we are done setting up the ActionBarSherlock as an IntelliJ library. Now, we will create the actual project.





 





## Creating your main project:





1. Select **File > New Project** and select **Application Module**.





2. Provide **Project name**. Let’s call it _MyAwesomeAndroidApp_ for the sake of this post.





3. Change **project SDK** to **Android 4.1.2 Platform**, and click **Next**.





4. In the next screen, again, for the sake of this post, change the **Activity name** to _MainActivity_ and click **Finish**.





5. From the next dialog that pops up, select **This Window** (it really doesn’t matter which one you choose).





6. Let’s rebuild the project once – **Build > Rebuild Project**





7. Open up **AndroidManifest.xml** file and change the min sdk version to 10, and set target sdk version to 16. The manifest should look something like:




    
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
              package="com.example.MyAwesomeAndroidApp"
              android:versionCode="1" android:versionName="1.0">
        <uses-sdk android:minSdkVersion="10" android:targetSdkVersion="16"/>
        <application android:label="@string/app_name" android:icon="@drawable/ic_launcher">
            <activity android:name=".MainActivity_" android:label="@string/app_name">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN"/>
                    <category android:name="android.intent.category.LAUNCHER"/>
                </intent-filter>
            </activity>
        </application>
    </manifest>





8. Try running your app now in an emulator. Your app should compile and run without any errors.





9. From the project selector on the left, select the _MyAwesomeAndroidApp_ project, and go to **File > Project Structure.**





10. Change the **Project language level** to **6.0 - @Override in interfaces**.





11. Select **Modules** on the left, and from the header of middle pane, click **+ **, and select **Import Module**.





12. Go to the _ActionBarSherlock_ project that we created before, and select the **ActionBarSherlock.iml** file. **Do not select the folder, just the .iml file.**





[![](http://www.ashokgelal.com/wp-content/uploads/2012/12/abs_add_iml-300x209.png)](http://www.ashokgelal.com/wp-content/uploads/2012/12/abs_add_iml.png)





13. Click **OK**. You will be asked to restart the IDE. Just click **YES**. After restarting, run your application again to make sure that the new changes haven’t cause you any harm.





 





## Using ActionBarSherlock:





1. Open _MainActivity.java_ and instead of extending _Activity_, extend _SherlockActivity_. IntelliJ should intelligently ask you to **Add dependency on module ‘ActionBarSherlock’**. If not, try hitting _ALT+Enter_





[![](http://www.ashokgelal.com/wp-content/uploads/2012/12/add_dependency_on_abs-300x83.png)](http://www.ashokgelal.com/wp-content/uploads/2012/12/add_dependency_on_abs.png)





2. Try running your app again. You should get a compiler error about _duplicate R.java file_. To fix, expand _ActionBarSherlock_ project, right click on the **target** folder, and from the context menu select **Mark Directory As > Excluded**.





[![](http://www.ashokgelal.com/wp-content/uploads/2012/12/mark_directory_excluded-280x300.png)](http://www.ashokgelal.com/wp-content/uploads/2012/12/mark_directory_excluded.png)





This should fix the duplicate file error but you will get another compiler error if you try to run it - _UNEXPECTED TOP-LEVEL EXCEPTION_. What the heck, right? The problem is that both ActionBarSherlock and our project is trying to load the same android support library. We need to tell ActionBarSherlock project that the support library will be provided, and it should not try to include and compile it. To fix this error, bring up the **Project Structure** window, (**File > Module Settings** or **F4**), select _ActionBarSherlock_ project, select **Dependencies** tab. From the list of dependencies, change the Scope of **android-support-v4.jar** to **Provided** from **Compile**. Run the app now. It should compile and run fine.





 





## Adding Maven Support:





Let’s convert our project to Maven project.





1.  Right click the _MyAwesomeAndroidApp_ project and select **Add Framework Support…** and from the list select **Maven**, and click **OK**.





2.  The editor will open the **pom.xml** file for you ready to change the **groupId**, **artifactId**, and **version**.





3.  Change **groupdId** to something like _com.example.myawesomeapp_, and **artifactId** to _MyAwesomeAndroidApp_.





If you look carefully, you will find that the project layout has slightly changed after you added the Maven support. The **src** folder now has a **main** and a **test** folder.





4.  Now, if you go to _MainActivity.java_ you will find red all over the place. It is because after we added the Maven support, it unlinked the ActionBarSherlock. For now _ALT+Enter_ and select **Add dependency on module ‘ActionBarSherlock’**. We will fix it permanently when we add dependencies to our *pom.xml *file.





5.  Run your app to be sure.





Now we will add the a[ndroid maven plugin](http://code.google.com/p/maven-android-plugin/) to our _pom.xml_ file.





6.  Add the following just before the closing `<project>` tag.




    
    <build>
        finalName>${project.artifactId}</finalName>
        <plugins>
            <plugin>
                <!-- See http://code.google.com/p/maven-android-plugin/ -->
                <groupId>com.jayway.maven.plugins.android.generation2</groupId>
                <artifactId>android-maven-plugin</artifactId>
                <version>3.4.1</version>
                <configuration>
                    <sdk>
                        <platform>8</platform>
                    </sdk>
                </configuration>
                <extensions>true</extensions>
            </plugin>
        </plugins>
    </build>





## Adding Roboelectric:





We will now add support for Roboelectric as well as jUnit for unit testing our project.





1. Add the following dependencies just before the opening `<build>` tag.





[frameworkads ad="1"]




    
    <dependencies>
            <dependency>
                <groupId>com.pivotallabs</groupId>
                <artifactId>robolectric</artifactId>
                <version>1.1</version>
                <scope>test</scope>
            </dependency>
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>4.10</version>
                <scope>test</scope>
            </dependency>
            <dependency>
                <groupId>com.actionbarsherlock</groupId>
                <artifactId>actionbarsherlock</artifactId>
                <version>4.2.0</version>
            </dependency>
    </dependencies>





2. Also, let’s add a unit test for the _MainActivity_ class. Go to _MainActivity.java_ class, **Alt+Enter **on the _MainActivity_ (the class name), and select **Create Test**. 3. Change the **Testing library** to **JUnit4** and click **OK**. Select **OK** on the next dialog. The editor should open _MainActivityTest.java_ class ready for adding your test. 4. Replace the content of _MainActivityTest.java_ with the following:




    
    package com.example.MyAwesomeAndroidApp;
    
    import com.xtremelabs.robolectric.RobolectricTestRunner;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    
    import static org.hamcrest.CoreMatchers.equalTo;
    import static org.junit.Assert.assertThat;
    
    @RunWith(RobolectricTestRunner.class)
    public class MainActivityTest {
    
         @Test
         public void shouldHaveProperAppName() throws Exception{
              String appName = new MainActivity().getResources().getString(R.string.app_name);
              assertThat(appName, equalTo("MyAwesomeAndroidApp"));
         }
    }





3. Within the test file, **right click**, select **Run > MainActivityTest**





4. At this point if you get a _!!! JUnit version 3.8 or later expected *error than the JUnit 3 library is getting precedence over JUnit 4. To fix this, bring the *module settings dialog_ (**File > Module Settings** or **F4**), select _MyAwesomeAndroidApp_ module, select **Dependencies** tab, select **Maven: junit:junit:4.10** entry and clicking the up arrow at the bottom, move it to the top – before the **Android 4.1.2 Platform** entry.





5. Try running the test again. The test should pass with few _binding shadow class warnings_. If you look closely, all these warnings point to some kind of Map related APIs. This means _Roboelectric_ cannot find the _Google maps API library_. To fix this, add the following dependency in your **pom.xml **file just before the _roboelectric_ dependency entry:




    
    <dependency>
           <groupId>com.google.android.maps</groupId>
           <artifactId>maps</artifactId>
           <version>17_r1</version>
           <scope>provided</scope>
    </dependency





6. Run the test again. You should get no warnings.





## Adding AndroidAnnotations:





Before you start, first find out where the maven repository is on your local computer. On my Mac, it is under _~/.m2_ which maps to _/Users/ashokgelal/.m2_.





7. Start by adding AndroidAnnotations Maven dependencies. Add following to your **pom.xml** file before the closing `<dependencies>` tag:




    
    <dependency>
        <groupId>com.googlecode.androidannotations</groupId>
        <artifactId>androidannotations</artifactId>
        <version>2.7</version>
        <scope>provided</scope>
    </dependency>
    
    <dependency>
        <groupId>com.googlecode.androidannotations</groupId>
        <artifactId>androidannotations-api</artifactId>
        <version>2.7</version>
    </dependency>





8. Open _IntelliJ IDEA preferences/ settings_ (on Mac, **IntelliJ > Preferences…** or **cmd + ,**)





9.  From the Settings dialog, expand **Compiler** and select **Annotation Processors**





10. The middle pane should list two projects. Select _MyAwesomeAndroidApp_.





11. From the right-most panel, check **Enable annotation processing**.





12. Select **Processor path:** radio button and provide this value for the path:




    
    
    <YOUR .m2 PATH>/repository/com/googlecode/androidannotations/androidannotations/2.7/androidannotations-2.7.jar:<YOUR .m2 PATH>/repository/com/googlecode/androidannotations/androidannotations-api/2.7/androidannotations-api-2.7.jar:<YOUR .m2 PATH>/repository/com/sun/codemodel/codemodel/2.4.1/codemodel-2.4.1.jar
    





Don’t forget to replace **_`<YOUR .m2 PATH>`_** with the path to your local _.m2_ repo.





13. For the **Store generated sources relative to:** option, select **Module content root** radio button.





14. For the **Production sources directory**, type _gen/aa_





15. For the **Test sources directory**, type _gen/aa-test_





[![](http://www.ashokgelal.com/wp-content/uploads/2012/12/annotation_processors-300x233.png)](http://www.ashokgelal.com/wp-content/uploads/2012/12/annotation_processors.png)





16. Replace the contents of _MainActivity.java_ with the following:




    
    import com.actionbarsherlock.app.SherlockActivity;
    import com.googlecode.androidannotations.annotations.EActivity;
    
    @EActivity(R.layout.main)
    public class MainActivity extends SherlockActivity {
    }





17. Rebuild the project.





18. Open the **AndroidManifest.xml** file and changed the **activity android:name** from _MainActivity_ to _MainActivity__





19.  Run your app. If a dialog box appears telling you that MainActivity is not declared in AndroidManifest.xml, change the **Launch** text to: _com.example.MyAwesomeAndroidApp.MainActivity__ If it still doesn’t work. Rebuild your project again before running it.





This seems a lot of work but believe me, after you do it couple of times, it is not that bad. I tried almost all the kickstarter/ bootstrapper projects and none of them worked. Also, they added a lot of extra stuff into my projects that I'm never going to need.





**BTW, have your heard about [LightPaper](http://www.ashokgelal.com/2013/02/light-paper-made-just-for-android/)?**





You can [follow me on Twitter](http://twitter.com/ashokgelal), or [add me on Google+](https://plus.google.com/102672078908622237427/posts). For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/android-development/official-tutorials/setting-up-intellij-idea-12-with-maven-actionbarsherlock-roboelectric-androidannotations/).






  [frameworkads ad="2"]




