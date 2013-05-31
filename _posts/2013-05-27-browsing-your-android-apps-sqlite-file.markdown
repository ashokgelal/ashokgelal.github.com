---
author: admin
comments: false
date: 2013-05-27 20:27:58+00:00
layout: post
comments: true
slug: browsing-your-android-apps-sqlite-file
title: Browsing your Android App's SQLite file
wordpress_id: 852
categories:
- android
- programming
tags:
- android
- database
- sqlite
---

There is no easy way to see the contents of your app's database in Android unless your device is rooted. If it is not rooted, you can first copy your database file to a sdcard (whether real or simulated) and then copy it back to your computer. You can then use any SQLite browser app such as [Firefox's SQLite Manager](https://addons.mozilla.org/en-us/firefox/addon/sqlite-manager/) to browse the contents. These are the steps you need:


1\. **Add the following helper function (doesn't matter where you put it; it's a static function):**

{% highlight java linenos %}

public static void copyDatabase(String packageName, String dbName) {
    try {
        File dbFile = new File(String.format("/data/data/%s/databases/%s", packageName, dbName));
        if (dbFile.exists()) {
            File dbDestFile = new File(String.format("%s/%s", Environment.getExternalStorageDirectory().getAbsoluteFile(), dbName));
            dbDestFile.createNewFile();
            InputStream in = new FileInputStream(dbFile);
            OutputStream out = new FileOutputStream(dbDestFile);
            byte[] buf = new byte[1024];
            int len;
            while ((len = in.read(buf)) > 0) {
                out.write(buf, 0, len);
            }
            in.close();
            out.close();
        }
    } catch (Exception e) {
        Log.e("database_copy", e.getLocalizedMessage());
    }
}
    
{% endhighlight %}
   

2\. **Call the above function somewhere from your main activity:**

{% highlight java linenos %}

…
    
  copyDatabase(getPackageName(), "yourdatabasename.db");
    
…
    
{% endhighlight %}


Replace **"yourdatabasename.db"** to the name of your own database.

3\. **The above 2 steps just copies your database file to your device's sdcard. You need to copy this file now back to your computer. We'll use `adb pull` to do it:**

`adb pull /sdcard/yourdatabasename.db ~/Desktop`

> Again, don't forget to change **yourdatabasename.db** to the name of your own database.

The database file should now be copied to your desktop.


You can [follow me on Twitter](https://twitter.com/ashokgelal), or [add me on Google+](https://plus.google.com/102672078908622237427/posts).
    