---
author: admin
comments: true
date: 2013-01-24 02:41:09+00:00
layout: post
slug: writing-a-real-android-app-from-scratch-part-39-gps-locationmanager-and-geocoding
title: 'Writing a Real Android App from Scratch: Part 3/9 – GPS, LocationManager and
  Geocoding'
wordpress_id: 772
categories:
- android
- programming
- tutorial
tags:
- android
- geocoding
- gps
- locationmanager
- programming
- tagsnap_tutorial
- tutorial
---

Welcome to part 3 of the [Writing a Real Android App from Scratch][1] series. 

In the [previous part][3] we added three containers for hosting our contents. The first of the contents is a view that displays current address info, which we will implement in this part. The idea is to fetch the current location (latitude and longitude), wait for the location to arrive, ask a 'translator' to translate the latitude and longitude to something that we can easily comprehend such as city, state, country, zip code etc, wait for the translator to give us the human-readable address, and eventually display the address. This will also be our first interaction with the underlying hardware - GPS radio and Wi-Fi radio. When hardware is involved, expect some delays to complete your requests. That's why we have to wait for each response after making a request. We will see how to perform/ simulate a 'wait' in Android.

This part is going to be a bit longer and a bit involved than the previous one. Again, if you got stuck on anything, don't hesitate to ask for help.

## Prerequisite:

*   Read previous parts of this series:
    
    *   [Part 0 - The World Beyond the Hello, World!][4]
    *   [Part 1 - About the App][5]
    *   [Part 2 - Tabs and Fragments][3]

*   Read the official Android tutorial on [making an app location aware][6].

*   Do some readings on [LocationManager][7].
*   Clone [this project from GitHub][8]; start with the `v0.2` tag:
    
        git checkout –b current_tab v0.2
        

## Plan of Action:

*   Write a reusable `LocationService` class that runs on a timer and calls us back as and when current location is available.
*   Add a menu item to the Action Bar for initiating a request to fetch the current location. 
*   Add required widgets to **CURRENT** fragment for displaying current address, and a button that will (in the next part) bring up another view for adding details and a picture.
*   After a location is available, translate the latitude and longitude coordinates to human-readable address.

## Fetching the Current Location:

We need a `LocationService` class that is responsbile for serving us the current location. It has to do so without blocking our main thread. You might need something like this class in your future projects as well. So, we will put some extra efforts to make this class reusable.

1\. **Create a new interface `LocationResultListener` with just one public that takes a single parameter of type `Location`:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/listeners/LocationResultListener.java
public interface LocationResultListener {
    public void onLocationResultAvailable(Location location);
}
{% endhighlight %}

The idea here is to have the `CurrentFragment` implement this interface so that when the location is available, we get back the result asynchronously. We will make `CurrentFragment` implement this interface later. For now let’s create the actual service class that is responsible for giving us back the location.

2\. **Add a new class `LocationService` and add a method `getLocation()`:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/services/LocationService.java
public class LocationService {
    private LocationResultListener mLocationResultListener;
    public boolean getLocation(Context context, LocationResultListener locationListener) {
        mLocationResultListener = locationListener; 
        return false;
    }
}
{% endhighlight %}

Before we move on to fill up the `getLocation()` method, let’s talk a little bit about what and how we want to get the location. In Android (and almost all of the other mobile platforms), there are two ways of getting a device's current location – with the help of a *GPS* radio and/ or with the help of a *Wi-Fi* radio. A GPS's location is more accurate than a Wi-Fi's but GPS might not be always available. Which means we need to make two requests for getting current location. We will do so by creating two instances of `LocationListener` and listen to `OnLocationChanged()` callback. However, you shouldn't be too optimist about these requests. What if the location is not currently available because, may be, both radios are off? Or there are no GPS signals? For such cases, we will try to get last known locations from both these providers and return the latest one. We will do this with a timer with some delay -- as we want to give getting current location a try, and get the last known location. This will also free the UI thread. In any case -- location immediately available or running timer to get the last known locations -- when we receive the location from either of the two providers, we will cancel the other one, stop the timer, and call the `LocationResultListener` passing the actual location. Let’s implement all these one by one. We will start with the constructor where we will create two instances of `LocationListener`:

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/services/LocationService.java
...
    private final LocationListener mGpsLocationListener;
    private final LocationListener mNetworkLocationListener;
    private LocationResultListener mLocationResultListener;
    private LocationManager mLocationManager;
    private Timer mTimer;

    public LocationService() {
        mGpsLocationListener = new LocationListener() {
            @Override
            public void onLocationChanged(Location location) {
                mTimer.cancel();
                mLocationManager.removeUpdates(this);
                mLocationManager.removeUpdates(mNetworkLocationListener);
                mLocationResultListener.onLocationResultAvailable(location);
            }

            @Override
            public void onStatusChanged(String s, int i, Bundle bundle) {
            }

            @Override
            public void onProviderEnabled(String s) {
            }

            @Override
            public void onProviderDisabled(String s) {
            }
        };

        mNetworkLocationListener = new LocationListener() {
            @Override
            public void onLocationChanged(Location location) {
                mTimer.cancel();
                mLocationManager.removeUpdates(this);
                mLocationManager.removeUpdates(mGpsLocationListener);
                mLocationResultListener.onLocationResultAvailable(location);

            }

            @Override
            public void onStatusChanged(String s, int i, Bundle bundle) {
            }

            @Override
            public void onProviderEnabled(String s) {
            }

            @Override
            public void onProviderDisabled(String s) {
            }
        };
    }
...
{% endhighlight %}

All we did was to create two instances of `LocationListener` and overrode the required methods. Inside `onLocationChanged()` callback, we cancel the timer, remove both the location listeners from getting any future updates (after all, we need the address only once), and set the result.

Now, we are ready to fill the `getLocation()` method. We don’t want to blindly request location updates from the provider before making sure that at least one of them is available. If only one of the providers is enabled then it’s okay but if both of them are disabled or not available, we will return a false indicating a failure.

3\. **Change the `getLocation()` method to:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/services/LocationService.java
...
    private boolean mGpsEnabled;
    private boolean mNetworkEnabled;

    public boolean getLocation(Context context, LocationResultListener locationListener) {
          mLocationResultListener = locationListener;
          if (mLocationManager == null)
              mLocationManager = (LocationManager) context.getSystemService(Context.LOCATION_SERVICE);

          try {
              mGpsEnabled = mLocationManager.isProviderEnabled(LocationManager.GPS_PROVIDER);
          } catch (Exception ex) { }

          try {
              mNetworkEnabled = mLocationManager.isProviderEnabled(LocationManager.NETWORK_PROVIDER);
          } catch (Exception ex) { }

          if (!mGpsEnabled && !mNetworkEnabled)
              return false;

          if (mGpsEnabled)
              mLocationManager.requestLocationUpdates(LocationManager.GPS_PROVIDER, 0, 0, mGpsLocationListener);

          if (mNetworkEnabled)
              mLocationManager.requestLocationUpdates(LocationManager.NETWORK_PROVIDER, 0, 0, mNetworkLocationListener);

          mTimer = new Timer();
          mTimer.schedule(new LastLocationFetcher(), 20000);
          return true;
    }
...
{% endhighlight %}

Now, all that is required is the `LastLocationFetcher` class that extends `TimerTask`. In the overridden `Run()` method we will first remove both listeners from receiving any updates (after all, the timer runs after 20 seconds, which means we gave up on getting the current location and falling back to last location). We will then check the latest of the two locations and set the result.

4\. **Add a private inner class `LastLocationFetcher`, make it extend `TimerTask`, and override `Run()` method:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/services/LocationService.java
...
    private class LastLocationFetcher extends TimerTask {

        @Override
        public void run() {

            // remove GPS location listener
            mLocationManager.removeUpdates(mGpsLocationListener);
            // remove network location listener
            mLocationManager.removeUpdates(mNetworkLocationListener);

            Location gpsLoc = null, netLoc = null;

            // if we had GPS enabled, get the last known location from GPS provider
            if (mGpsEnabled)
                gpsLoc = mLocationManager.getLastKnownLocation(LocationManager.GPS_PROVIDER);

            // if we had WiFi enabled, get the last known location from Network provider
            if (mNetworkEnabled)
                netLoc = mLocationManager.getLastKnownLocation(LocationManager.NETWORK_PROVIDER);


            // if we had both providers, get the newest last location
            if (gpsLoc != null && netLoc != null) {
                if (gpsLoc.getTime() > netLoc.getTime())
                    // last location from the GPS provider is the newest
                    mLocationResultListener.onLocationResultAvailable(gpsLoc);
                else
                    // last location from the Network Provider is the newest
                    mLocationResultListener.onLocationResultAvailable(netLoc);
                return;
            }

            // looks like network provider is not available
            if (gpsLoc != null) {
                mLocationResultListener.onLocationResultAvailable(gpsLoc);
                return;
            }

            // looks like GPS provider is not available
            if (netLoc != null) {
                mLocationResultListener.onLocationResultAvailable(netLoc);
                return;
            }

            mLocationResultListener.onLocationResultAvailable(null);
        }
    }
... 
{% endhighlight %}

That’s all you need to create a reusable location service class that you can, if you want, use in your other projects.

We now need a button that our user can select to get the location. We will achieve this by adding a menu item to the Action Bar.

5\. **Under the `res` folder, create a new folder - `menu`.**

6\. **Under the `res/menu` folder, create a new menu resource file – `current_location.xml`:**

{% highlight xml linenos %}<!-- file: res/layout/current_location.xml -->
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/find_current_location"
        android:icon="@drawable/current_location"
        android:showAsAction="always"/>
</menu>
{% endhighlight %}

7\. **Open `CurrentFragment.java`, override three methods – `onActivityCreated()`, `onCreateOptionsMenu`, and `onOptionsItemSelected()`. The last two methods are required to make the menu item work:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/CurrentFragment.java
...
    @Override
    public void onActivityCreated(Bundle savedInstanceState) {
        super.onActivityCreated(savedInstanceState);
        setHasOptionsMenu(true);
    }

    @Override
    public void onCreateOptionsMenu(Menu menu, MenuInflater inflater) {
        inflater.inflate(R.menu.current_location, menu);
        super.onCreateOptionsMenu(menu, inflater);
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        if (item.getItemId() == R.id.find_current_location) {
            // user wants to fetch the location
            return true;
        }
        return super.onOptionsItemSelected(item);
    }
...
{% endhighlight %}

Inside the `onOptionsItemSelected()` method, we check the id of the menu item that is selected. If it is of the `find_current_location` menu item, we need to fetch the location. But first, run the app. You should see a GPS icon on the right hand side of the Action Bar.

<img src="https://dl.dropbox.com/u/83257/Tagsnap_Screenshots/ss6_GpsIcon.png" Width="40%" />

8\. **Let’s call our location service from within the `onOptionsItemSelected()` method:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/CurrentFragment.java
...
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        if (item.getItemId() == R.id.find_current_location) {
            if (mLocationService == null)
                mLocationService = new LocationService();
            mLocationService.getLocation(getActivity(), this);
            return true;
        }
        return super.onOptionsItemSelected(item);
    }
...
{% endhighlight %}

We lazily created an instance of `LocationService` class that we wrote earlier and call `getLocation()` method. We are passing `this` as the last parameter of `getLocation()` method, which means our `CurrentFragment` class has to implement `LocationResultListener.` Let’s do that.

9\. **Make CurrentFragment implement `LocationResultListener`, and override the only `onLocationResultAvailable()` method:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/services/LocationService.java
...
    @Override
    public void onLocationResultAvailable(Location location) {
        if (location == null) {
             // TODO: [ASSIGNMENT]
        } else {
            // reverse geocode location
        }
    }
...
{% endhighlight %}

## Getting ready to display the address:

Now that we have the location with latitude and longitude, next task is to reverse geocode this location to get the human-readable address. But first let’s add the widgets necessary to display this human-readable address. We will start by adding a layout for `CurrentFragment`.

10\. Under `res/layout` folder, create a new layout resource file – `current_frag.xml`, and replace the contents with:

{% highlight xml linenos %} <!-- file: res/layout/current_frag.xml -->
    <LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:orientation="vertical"
        android:weightSum="100">        
        
        <RelativeLayout
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:layout_weight="50">
            <ImageView
                android:id="@+id/locationIcon"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:src="@drawable/unknown_location"
                android:layout_centerVertical="true"
                android:layout_marginLeft="40dip"/>
            <TextView
                android:id="@+id/address1"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_toRightOf="@+id/locationIcon"
                android:layout_alignTop="@+id/locationIcon"
                android:layout_marginLeft="30dip"
                android:textColor="#0099cc"
                android:textSize="22sp"
                android:layout_alignParentTop="false"
                android:layout_marginTop="5dip"/>
            <TextView
                android:id="@+id/address2"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:textColor="#0099cc"
                android:textSize="18sp"
                android:layout_alignLeft="@+id/address1"
                android:layout_below="@+id/address1"/>

            <TextView
                android:id="@+id/latitude"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:textColor="#dddddd"
                android:textSize="14sp"
                android:layout_alignLeft="@+id/address1"
                android:layout_below="@+id/address2"/>

            <TextView
                android:id="@+id/longitude"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:textColor="#dddddd"
                android:textSize="14sp"
                android:layout_alignLeft="@+id/address1"
                android:layout_below="@+id/latitude"/>

        </RelativeLayout>

        <ImageButton
            android:id="@+id/tagButton"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="0dp"
            android:layout_weight="0"
            android:src="@drawable/tag_button_disabled"
            android:clickable="false"
            android:layout_centerInParent="true"
            android:layout_gravity="center_horizontal"/>

    </LinearLayout>
    
{% endhighlight %}

There is no rocket science here. All we did was to add a bunch of text views and an image view for displaying different address fields. We also added an image button that the user, after we finish it in the next part, can select to add details and a picture for the ‘snapped’ address.

Over in `CurrentFragment` let’s set this layout as the root view and save pointers to all the widgets that we need to reference later.

11\. **Override `onCreateView()` method in `CurrentFragment`:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/CurrentFragment.java
...
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        return inflater.inflate(R.layout.current_frag, container, false);
    }
...
{% endhighlight %}

12\. **Modify `onActivityCreated()` method to:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/CurrentFragment.java
... 
    private ImageView mLocationIcon;
    private ImageButton mTagButton;
    private TextView mAddress1;
    private TextView mAddress2;
    private TextView mLat;
    private TextView mLon;

    @Override
    public void onActivityCreated(Bundle savedInstanceState) {
        super.onActivityCreated(savedInstanceState);
        setHasOptionsMenu(true);
        mLocationIcon = (ImageView) getView().findViewById(R.id.locationIcon);
        mTagButton = (ImageButton) getView().findViewById(R.id.tagButton);
        mAddress1 = (TextView) getView().findViewById(R.id.address1);
        mAddress2 = (TextView) getView().findViewById(R.id.address2);
        mLat = (TextView) getView().findViewById(R.id.latitude);
        mLon = (TextView) getView().findViewById(R.id.longitude);
    }
... 
{% endhighlight %}

Run the app, the **CURRENT** tab should show one image, a red 'pin', and a grayed out image button.

## Reverse Geocoding the Location:

Now that we have the widgets necessary to show the address details, let’s translate the location, which we got from the `LocationService` class earlier, to a human-readable address. Just like getting the location from the location providers, reverse geocoding takes some time to return you the result. You don’t want to freeze your UI thread during this time otherwise Android may kill your app thinking it is frozen. The better option is to have it extend an [AsyncTask][9] and do the geocoding task in the background. After the geocoding is done, it can call the `CurrentFragment` back passing the actual address (or null if it cannot geocode the location). You might have guessed it already; we are going to need another interface for this `AsyncTask` to be able to call back the `CurrentFragment`. Let’s implement that interface first.

13\. **Add a new interface `ReverseGeocodingListener`, and a public method `onAddressAvailable()`:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/listeners/ReverseGeocodingListener.java
public interface ReverseGeocodingListener {
    public void onAddressAvailable(Address address);
}

{% endhighlight %}

14\. **Add a new class `ReverseGeocodingService` that extends `AsyncTask<Location, Void, Void>`, and override `doInBackground()` method:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/services/ReverseGeocodingService.java
public class ReverseGeocodingService extends AsyncTask<Location, Void, Void> {
    @Override
    protected Void doInBackground(Location... locations) {
        // do actual geocoding
        return null;
    }
}
{% endhighlight %}

15\. **Add a constructor that takes two parameters - a `Context` and a `ReverseGeocodingListener`:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/services/ReverseGeocodingService.java
...
    private final ReverseGeocodingListener mListener;
    private final Context mContext;

    public ReverseGeocodingService(Context context, ReverseGeocodingListener listener) {
        mContext = context;
        mListener = listener;
    }
...
{% endhighlight %}

16\. **Modify `doInBackgroundMethod()`:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/services/ReverseGeocodingService.java
...
    private Address mAddress;

    @Override
    protected Void doInBackground(Location... locations) {
        // create an instance of a Geocoder
        Geocoder geocoder = new Geocoder(mContext, Locale.getDefault());

        // get the first location
        Location loc = locations[0];
        // a location might be associated with multiple addresses; so we need a list
        List<Address> addresses = null;

        try {
            // ask the Geocoder to give a list of address for the given latitude and longitude
            // 1 means max result - we need only 1
            addresses = geocoder.getFromLocation(loc.getLatitude(), loc.getLongitude(), 1);
        } catch (IOException e) {
            mAddress = null;
        }

        // get the first address
        if (addresses != null && addresses.size() > 0) {
            mAddress = addresses.get(0);
        }

        return null;
    }
...
{% endhighlight %}

We created a new instance of `Geocoder` class and get all the addresses for the given latitude and longitude. We are only interested in the first address so we set the last parameter, `MaxResult`, to 1. If we get back one, we save it to a field.

We cannot call `onAddressAvailable()` method from within `doInBackground()` method because this method is running on a different thread, and you cannot touch the widgets from any other threads than the one that created it -- in our case the UI thread. We will do that from a different method that is guaranteed to be run on the UI thread – `onPostExecute()`.

17\. **Override `onPostExecute()` method:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/services/ReverseGeocodingService.java
...
    @Override
    protected void onPostExecute(Void aVoid) {
        mListener.onAddressAvailable(mAddress);
    }
...
{% endhighlight %}

That’s all we need to reverse geocode a location. Next task is to call this `AsyncTask` from within our `CurrentFragment`.

18\. **Open `CurrentFragment` and modify `onLocationResultAvailable()` method:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/CurrentFragment.java
...
    @Override
    public void onLocationResultAvailable(Location location) {
        if (location == null) {
            // TODO: [ASSIGNMENT]
        } else {
            new ReverseGeocodingService(getActivity(), this).execute(location);
        }
    }
...
{% endhighlight %}

19\. **Make `CurrentFragment` implement `ReverseGeocodingListener`:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/CurrentFragment.java
public class CurrentFragment extends SherlockFragment implements LocationResultListener, ReverseGeocodingListener {

    ...
}
{% endhighlight %}

20\. **Override the required `onAddressAvailable()` method:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/CurrentFragment.java
...
    private Address mLastKnownAddress;
    @Override
    public void onAddressAvailable(Address address) {
      if (address == null) {
          // TODO: [ASSIGNMENT]
      } else {
        mLastKnownAddress = address;
        setAddressDetails(address);
      }
    }
...
{% endhighlight %}

21\. **Add a new method - `setAddressDetails()`:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/CurrentFragment.java
...
    private void setAddressDetails(Address address) {
      if (address.getMaxAddressLineIndex() > 0)
          mAddress1.setText(address.getAddressLine(0));

      mAddress2.setText(String.format("%s, %s, %s", address.getLocality(), address.getAdminArea(), address.getCountryName()));
      Resources res = getResources();
      mLat.setText(String.format(res.getString(R.string.lat_val), address.getLatitude()));
      mLon.setText(String.format(res.getString(R.string.lon_val), address.getLongitude()));

      mTagButton.setImageResource(R.drawable.tag_button);
      mLocationIcon.setImageResource(R.drawable.known_location);
      mTagButton.setClickable(true);
    }
...
{% endhighlight %}

22\. **Add two new string resources to `res/values/strings.xml` file:**

{% highlight xml linenos %}<!-- file: res/values/strings.xml -->
...
    <string name="lat_val">Lat: %1$f </string>
    <string name="lon_val">Lon: %1$f </string>
...
{% endhighlight %}

Is that all we need to get the location, reverse geocode it and display the address? Run it, click the GPS menu item, and see it yourself. Not quite! You will get an exception. You need to get [permission to use any protected Android features][10] such as locations, reverse geocoding (you actually don’t need permission to use reverse geocoding, but you do need *Internet* to perform geocoding, so you need permission to use Internet instead). You do so by adding `<uses-permission>` tag for each of the features that your app requires in the app's `AndroidManifest.xml` file.

23\. **Open AndroidManifest.xml file, and add the following tags right after the <uses-sdk ... > tag:**

{% highlight xml linenos %}<!-- file: AndroidManifest.xml -->
...
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.INTERNET"/>
... 
{% endhighlight %}

Run your app again, click the GPS menu item, and wait few seconds to see your current address being displayed. If you are using an emulator for debugging then it might not work. I never got reverse geocoding to work on an Android emulator.

## Saving and Restoring the Address on Config Changes:

Similar to the selected tab index problem we had in the previous part, the **CURRENT** tab's widget values reset back when you change the orientation mode. Try it. We are going to fix this problem using the same technique we used before – saving the current address in `onSaveInstanceState()` callback and restoring it from `onActivityCreated()` method. The `Address` class implements `android.os.Parcelable` making it easier to put it in a `Bundle`.

24\. **Override `onSaveInstanceState()` method:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/CurrentFragment.java
...
    @Override
    public void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        if(mLastKnownAddress != null)
            outState.putParcelable("last_known_address", mLastKnownAddress);
    }
...
{% endhighlight %}

25\. **Now to restore the address, append following lines at the end of the `onActivityCreated()` method:**

{% highlight java linenos %}//file: src/main/java/com.ashokgelal.tagsnap/main/CurrentFragment.java
...
  ...
      if (savedInstanceState != null)
        mLastKnownAddress = savedInstanceState.getParcelable("last_known_address");
      if(mLastKnownAddress != null)
        setAddressDetails(mLastKnownAddress);
  ...
...
{% endhighlight %}

We have one final task remaining in `CurrentFragment` –- hooking up the big *TagButton* to take the user to a screen for adding a description, select category, and attach a picture. We already had enough to do in this part, so we will hook up the button as well as add the details screen in the next part of this series.

## Assignments:

1\.  I’ve left three **TODOs** in the code above all of which are about notifying the user when we don’t get the location or the human-readable address. Try to implement these on your own. To start with you can use a [Toast][11], or even better, an [AlertDialog][12].

2\.  After the user selects the *GPS* button from the Action Bar, the app doesn’t give any feedback about fetching the location and reverse geocoding it in the background. Use a [ProgressDialog][13], or even better, a [ProgressBar][14] to notify user about the on going operation.

We did a handful of coding today but don't let the total lines of code intimidate you. If you think about it, we not only displayed reverse geocoded address but also ended up writing a reusable `LocationService` class that you can use in other projects. 

In the next part, we will allow our user to add details. Among others, we will learn how to capture a picture from a device camera, as well as how to pick up a saved picture from the gallery. We will also learn how to modify our Action Bar by replacing its content with a completely new custom view.

You can [follow me on Twitter][15], or [add me on Google+][16].

See you in the next part.  


 [1]: http://www.ashokgelal.com/tag/tagsnap_tutorial/
 [3]: http://www.ashokgelal.com/?p=437
 [4]: http://www.ashokgelal.com/?p=401
 [5]: http://www.ashokgelal.com/?p=417
 [6]: http://developer.android.com/training/basics/location/index.html
 [7]: http://developer.android.com/reference/android/location/LocationManager.htm
 [8]: https://github.com/ashokgelal/Tagsnap
 [9]: http://developer.android.com/reference/android/os/AsyncTask.html
 [10]: http://developer.android.com/guide/topics/security/permissions.html
 [11]: http://developer.android.com/guide/topics/ui/notifiers/toasts.html
 [12]: http://developer.android.com/reference/android/app/AlertDialog.html
 [13]: http://developer.android.com/guide/topics/ui/dialogs.html
 [14]: http://developer.android.com/reference/android/widget/ProgressBar.html
 [15]: https://twitter.com/ashokgelal
 [16]: https://plus.google.com/102672078908622237427/posts
