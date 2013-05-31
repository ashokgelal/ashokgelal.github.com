---
author: admin
comments: false
date: 2013-02-05 04:45:19+00:00
layout: post
slug: writing-a-real-android-app-from-scratch-part-49-camera-gallery-and-custom-action-bar
title: 'Writing a Real Android App from Scratch: Part 4/9 – Camera, Gallery, and Custom
  Action Bar'
wordpress_id: 789
categories:
- android
- programming
- tutorial
tags:
- ActionBar
- android
- camera
- gallery
- programming
- tagsnap
- tagsnap_tutorial
- tutorial
---

Welcome to part 4 of the [Writing a Real Android App from Scratch](http://www.ashokgelal.com/tag/tagsnap_tutorial/) series. For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/android-development/official-tutorials/writing-a-real-android-app-from-scratch-tagsnap/).





At this point, our app is capable of getting user’s current location and displaying the human-readable address for that location. Now, we need a way to add details to that location. The details, in our case, will contain a description, a category, and a picture. We already have the address so the user doesn’t have to enter any. Adding a description and a category is easy – we just need a text view, and a spinner view. Selecting a picture from gallery or taking a picture with the camera is more difficult, and more engaging. We have to lot to cover in this part - Gallery, camera, custom Action Bar, and other small details. I could have covered all of these one by one breaking into different tutorial parts but these are related, and we want to get one feature done in one part. So, this part the longest part of this tutorial series. Let's get started.





## Pre-requisite:







  * Read previous parts of this series:





    * [Part 0 - The World Beyond the Hello, World!](http://www.ashokgelal.com/?p=401)


    * [Part 1 - About the App](http://www.ashokgelal.com/?p=417)


    * [Part 2 - Tabs and Fragments](http://www.ashokgelal.com/?p=437)


    * [Part 3 - GPS, LocationManager, and Geocoding](http://wp.me/p36TSC-cs)




  * Do some readings on [Intents and Intent Filters](http://developer.android.com/guide/components/intents-filters.html).



  * Read [this Google+ post from Roman Nurik](https://plus.google.com/u/0/113735310430199015092/posts/R49wVvcDoEW) about implementing an _ActionBar_ with _Done+Discard_ menu items.


  * Clone [this project from GitHub](https://github.com/ashokgelal/Tagsnap); start with the `v0.3.1` tag:




    
    <code>git checkout –b add_details v0.3.1
    </code>






## Plan of Action:







  * Hook up the 'tag button' to take the user to a different activity.





We don’t want to make `CurrentFragment` decide what to do after user selects the tag button because in the future we will be saving the details to a database, and we don’t want to litter all our fragments with database related code. We will let `DefaultActivity` decide what it wants to do with the address that `CurrentFragment` provides.



  * Add a layout for adding details as well as add the corresponding activity.


  * Add support for picking up a gallery picture and for taking picture with a camera.


  * Add a way to discard or accept the details.





## Hooking up the tag button:





At the end of [previous part](http://wp.me/p36TSC-cs), we left hooking up the tag button that takes you to a different activity for adding details, and a picture. As discussed before, we want `DefaultActivity` to handle whatever it wants to do with the address. This means we will callback the `DefaultActivity` with the reverse geocoded address after the user selects the button. When I said callback, you might have guessed it already – we are going to use an interface that has just one public method – `onAddressAvailable()` and takes an _Address_ as a parameter. But wait! We already have that interface – `ReverseGeocodingListener`, which declares the exact same method we are after. The only problem is its name -- doesn't sound quite right for what we want to do. Let’s change it.





1. Rename `ReverseGeocodingListener` class name to `AddressResultListener`. Don’t forget to change the name of the file itself as well as all the references to this interface in `CurrentFragment`, and `ReverseGeocodingService` classes.





2. Make `DefaultActivity` implement `AddressResultListener` interface:




    
    //file: src/main/java/com.ashokgelal.tagsnap/DefaultActivity.java
    public class DefaultActivity extends SherlockFragmentActivity implements AddressResultListener {
    ...
        @Override
        public void onAddressAvailable(Address address) {
        }
    ...
    }
    





As we know, the root activity of our app is `DefaultActivity` and `CurrentFragment` is a child of it. The `DefaultActivity` knows about the `CurrentFragment` but the reverse is not true. How can we then callback the `DefaultActivity`? By enforcing the parent activity to implement the `AddressResultListener` interface, which we already did in step _1_ and _2_ above. Now, inside the `CurrentFragment` we need to make sure that the parent, whoever it may be, needs to implement the `AddressResultListener` interface. The best place to do this is inside an overridden `onAttach()` method.





3. Override `onAttach()` method in `CurrentFragment`:




    
    //file: src/main/java/com.ashokgelal.tagsnap/CurrentFragment.java
    ...
        private AddressResultListener mAddressResultListener;
    
        @Override
        public void onAttach(Activity activity) {
            super.onAttach(activity);
            try {
                mAddressResultListener = (AddressResultListener) activity;
            } catch (ClassCastException e) {
                throw new ClassCastException(activity.toString() + " must implement AddressResultListener");
            }
        }
    ...
    





Finally, we can now hook up the tag button. All we need to set the action listener on the button.





4. Inside `onActivityCreated()` method, add the following line right before where we check whether `savedInstanceState` is null or not:




    
    //file: src/main/java/com.ashokgelal.tagsnap/CurrentFragment.java
    ...
        public void onActivityCreated(Bundle savedInstanceState) {
            ...
            mTagButton.setOnClickListener(this);
            ...
        }
    ...
    





5. Make `CurrentFragment` implement `View.OnClickListener` interface, and then override `onClick():`




    
    //file: src/main/java/com.ashokgelal.tagsnap/CurrentFragment.java
    public class CurrentFragment extends SherlockFragment implements LocationResultListener, AddressResultListener, View.OnClickListener {
    ...
        @Override
        public void onClick(View view) {
            if(mAddressResultListener != null && mLastKnownAddress != null)
                mAddressResultListener.onAddressAvailable(mLastKnownAddress);
        }
    ...
    }
    





That’s all we need to pass back the address to `DefaultActivity`. Before we leave `CurrentFragment` let’s finish off something that we haven’t yet talked about – stopping the Location Managers that we instantiated in the previous part. Imagine a scenario where we have fired up the `LastLocationFetcher` timer and waiting for the last known location. During this waiting time the user decides to leave the app or switch to a different app. In this case, we need to make sure that we stop the timer and unregister from receiving any updates from either of the two radios.





6. In `LocationService` class, add a `stop()` method:




    
    //file: src/main/java/com.ashokgelal.tagsnap/services/LocationService.java
    ...
       public void stop() {
          if (mTimer != null)
              mTimer.cancel();
          mLocationManager.removeUpdates(mGpsLocationListener);
          mLocationManager.removeUpdates(mNetworkLocationListener);
       }
    ...
    





If you want, you can also refactor out the two `onLocationChanged()` methods inside the `LocationService()` constructor to call the `stop()` method. Eventually, we need to call this `stop()` method from `CurrentFragment`.





7. In `CurrentFragment` class, override `onStop()` method:




    
    //file: src/main/java/com.ashokgelal.tagsnap/CurrentFragment.java
    ...
        @Override
        public void onStop() {
            super.onStop();
            if (mLocationService != null)
                mLocationService.stop();
        }
    ...
    





That’s all for hooking up the tag button.





## Adding details for current address:





We need an activity and a view for letting user add details and a picture. Let’s call this activity `DetailsActivity`. When the `DefaultActivity` receives an `Address` in `onAddressAvailable()` callback, we want to start the new `DetailsActivity` passing the address itself. When `DetailsActivity` is finished, we get back to the `DefaultActivity` again where we will extract the description, picture url, category information out of intent data. The recommended way of passing the data between different activities is putting each field into intent itself using key-value pairs. But this is ugly, as we need to remember the exact magic key strings when retrieving the values. A better approach is to have a model class and pass an instance of that model class instead. But this comes with another problem – you cannot just put any object to an `Intent`. All the primitive types are supported and few others. However, you can let your model class implement [Parcelable](http://developer.android.com/reference/android/os/Parcelable.html) to be able to put it to intent. We will go this route. First we need a model class; let’s call it `TagInfo`.





8. Create a new class `TagInfo`, and make it implement `Parcelable`. You also need to override few methods from `Parcelable`:




    
    //file: src/main/java/com.ashokgelal.tagsnap/model/TagInfo.java
    public class TagInfo implements Parcelable{
        @Override
        public int describeContents() {
            return 0;
        }
    
        @Override
        public void writeToParcel(Parcel parcel, int i) {
        }
    }
    





We will now add required fields as well as the setters and getters.





9. Modify `TagInfo` class to add the following fields, getters, setters:




    
    //file: src/main/java/com.ashokgelal.tagsnap/model/TagInfo.java
    public class TagInfo implements Parcelable{
    ...
    
        private String mDescription;
        private String mCategory;
        private Uri mPictureUri;
        private String mAddress1;
        private String mAddress2;
        private double mLatitude;
        private double mLongitude;
        private long mId;
    
        public long getId() {
            return mId;
        }
    
        public void setId(long id) {
            mId = id;
        }
    
        public String getDescription() {
            return mDescription;
        }
    
        public void setDescription(String description) {
            this.mDescription = description;
        }
    
        public String getCategory() {
            return mCategory;
        }
    
        public void setCategory(String category) {
            this.mCategory = category;
        }
    
        public Uri getPictureUri() {
            return mPictureUri;
        }
    
        public void setPictureUri(Uri pictureUri) {
            this.mPictureUri = pictureUri;
        }
    
        public void setPictureUri(String uri) {
            setPictureUri(Uri.parse(uri));
        }
    
        public String getAddress1() {
            return mAddress1;
        }
    
        public void setAddress1(String address1) {
            this.mAddress1 = address1;
        }
    
        public String getAddress2() {
            return mAddress2;
        }
    
        public void setAddress2(String address2) {
            this.mAddress2 = address2;
        }
    
        public double getLatitude() {
            return mLatitude;
        }
    
        public void setLatitude(double latitude) {
            this.mLatitude = latitude;
        }
    
        public double getLongitude() {
            return mLongitude;
        }
    
        public void setLongitude(double longitude) {
            this.mLongitude = longitude;
        }
    ...
    





10. Add 3 constructors; the one that takes a `Parcel` data is required to be able to parcel this class back and forth.




    
    //file: src/main/java/com.ashokgelal.tagsnap/model/TagInfo.java
    ...
        public TagInfo(long id) {
            mId = id;
        }
    
        public TagInfo() {
            this(-1);
        }
    
        public TagInfo(Parcel data) {
            setId(data.readLong());
            setDescription(data.readString());
            setCategory(data.readString());
            setPictureUri(data.readString());
            setAddress1(data.readString());
            setAddress2(data.readString());
            setLatitude(data.readDouble());
            setLongitude(data.readDouble());
        }
    ...
    





Again, notice the last constructor. We are passed in the `Parcel` data during deserialization of this class. The deserialization order **_has_** to match with that of the serialization, which we will do next.





11. Modify `describeMethods()` method and `writeToParcel()` methods to:




    
    //file: src/main/java/com.ashokgelal.tagsnap/model/TagInfo.java
    ...
        @Override
        public int describeContents() {
            // just returning the hash code should be enough
            return hashCode();
        }
    
        @Override
        // the orders of 'writing' should match that of 'reading'
        public void writeToParcel(Parcel parcel, int flags) {
            parcel.writeLong(getId());
            parcel.writeString(getDescription());
            parcel.writeString(getCategory());
            if (getPictureUri() == null)
                parcel.writeString("");
            else
                parcel.writeString(getPictureUri().getPath());
            parcel.writeString(getAddress1());
            parcel.writeString(getAddress2());
            parcel.writeDouble(getLatitude());
            parcel.writeDouble(getLongitude());
        }
    ...
    





We are almost done. The final piece that is missing is a static `Creator` for creating a `Parcelable` data of type `TagInfo` and another type of an array of `TagInfo`





12. Add a static final `Creator` field at the end of the class:.




    
    //file: src/main/java/com.ashokgelal.tagsnap/model/TagInfo.java
    ...
        public static final Creator<taginfo> CREATOR = new Creator<taginfo>() {
            @Override
            public TagInfo createFromParcel(Parcel parcel) {
                return new TagInfo(parcel);
            }
    
            @Override
            public TagInfo[] newArray(int size) {
                return new TagInfo[size];
            }
            
        };
    
    ...
    





Now an object of this class can be sent back and forward from one activity to another as a _Parcel_.





## Adding Details:





We want to pass an instance of `TagInfo` to `DetailsActivity`, which will be responsible for setting the description, category, and a picture uri for the `TagInfo` object, and then send it back.





13. Add a new layout `details.xml`, and replace the content with:




    
    
    <ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:id="@+id/scrollView">
    
        <RelativeLayout
            android:orientation="vertical"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content">
    
            <EditText
                android:id="@+id/description"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_alignParentLeft="true"
                android:layout_alignParentTop="true"
                android:layout_marginTop="5dip"
                android:hint="@string/description"/>
    
            <TextView
                android:id="@+id/categoryHeader"
                style="@style/HeaderText"
                android:text="@string/category"
                android:layout_alignLeft="@+id/description"
                android:layout_below="@+id/description"/>
    
            <View
                style="@style/HeaderTextSeparator"
                android:layout_below="@+id/categoryHeader"
                android:id="@+id/categorySeparator"/>
    
            <Spinner
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/category"
                android:layout_alignParentLeft="true"
                android:layout_below="@+id/categoryHeader"
                android:entries="@array/categories_array"
                android:layout_marginTop="4dip"
                android:layout_marginLeft="4dip"
                android:spinnerMode="dialog"
                android:layout_marginRight="4dip"/>
    
            <RelativeLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_alignLeft="@+id/description"
                android:layout_below="@+id/category"
                android:layout_marginTop="10dip"
                android:id="@+id/relativeLayout">
    
                <ImageView
                    android:layout_width="170dip"
                    android:layout_height="128dip"
                    android:id="@+id/previewImage"
                    android:src="@drawable/photo_preview"
                    android:layout_marginTop="10dip"
                    android:layout_marginLeft="10dip"/>
    
                <ImageButton
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:id="@+id/cameraButton"
                    android:src="@drawable/add_from_camera"
                    android:layout_toLeftOf="@+id/galleryButton"
                    android:layout_alignParentTop="false"
                    android:layout_centerVertical="true"
                    android:visibility="gone"/>
    
                <ImageButton
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:id="@+id/galleryButton"
                    android:src="@drawable/add_from_gallery"
                    android:layout_alignParentTop="false"
                    android:baselineAlignBottom="false"
                    android:layout_centerVertical="true"
                    android:layout_alignParentRight="true"
                    android:layout_marginRight="10dip"/>
            </RelativeLayout>
    
            <TextView
                style="@style/HeaderText"
                android:id="@+id/locationHeader"
                android:text="@string/location"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_alignLeft="@+id/relativeLayout"
                android:layout_below="@+id/relativeLayout"/>
    
            <View
                style="@style/HeaderTextSeparator"
                android:layout_below="@+id/locationHeader"
                android:id="@+id/locationSeparator"/>
    
            <TextView
                android:id="@+id/addressHeader"
                style="@style/LocationHeaderText"
                android:text="@string/address"
                android:layout_alignLeft="@+id/locationHeader"
                android:layout_below="@+id/locationHeader"/>
    
            <TextView
                android:id="@+id/detailsAddress1"
                style="@style/LocationDetailsText"
                android:layout_below="@+id/addressHeader"
                android:layout_alignLeft="@+id/addressHeader"/>
    
            <TextView
                android:id="@+id/detailsAddress2"
                style="@style/LocationDetailsText"
                android:layout_alignLeft="@+id/categoryHeader"
                android:layout_below="@+id/detailsAddress1"/>
    
            <View
                style="@style/LocationTextSeparator"
                android:layout_below="@+id/detailsAddress2"
                android:id="@+id/addressSeparator"/>
    
            <TextView
                style="@style/LocationHeaderText"
                android:id="@+id/latitudeHeader"
                android:text="@string/latitude"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_alignLeft="@+id/addressHeader"
                android:layout_below="@+id/addressSeparator"/>
    
            <TextView
                style="@style/LocationDetailsText"
                android:id="@+id/detailsLatitude"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_alignLeft="@+id/addressHeader"
                android:layout_below="@+id/latitudeHeader"/>
    
            <View
                style="@style/LocationTextSeparator"
                android:layout_below="@+id/detailsLatitude"
                android:id="@+id/latitudeSeparator"/>
    
            <TextView
                style="@style/LocationHeaderText"
                android:id="@+id/longitudeHeader"
                android:text="@string/longitude"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_alignLeft="@+id/addressHeader"
                android:layout_below="@+id/latitudeSeparator"/>
    
            <TextView
                style="@style/LocationDetailsText"
                android:id="@+id/detailsLongitude"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_alignLeft="@+id/addressHeader"
                android:layout_below="@+id/longitudeHeader"/>
    
            <View
                style="@style/LocationTextSeparator"
                android:layout_below="@+id/detailsLongitude"
                android:id="@+id/longitudeSeparator"/>
    
        </RelativeLayout>
    </ScrollView>
    





Here we just have a bunch of widgets for allowing our user to input details as well as text views for displaying address details.





14. Open `strings.xml` and add following resources:




    
    
        <string name="category">Category</string>
        <string name="address">Address</string>
        <string name="description">Description</string>
        <string name="location">Location</string>
        <string name="latitude">Latitude</string>
        <string name="longitude">Longitude</string>
    





15. We also need few pre-made categories. For now, let’s have 4 categories. You can add more if you want. Add an string array resource to `strings.xml` file:




    
    
        <string-array name="categories_array">
            <item>Musuem</item>
            <item>Park</item>
            <item>Theatre</item>
            <item>Historial Landmark</item>
        </string-array>
    





16. Create a new file `styles.xml` under `res/values/` and replace the content with:




    
    
    <resources>
        <style name="HeaderText">
            <item name="android:textSize">14sp</item>
            <item name="android:layout_width">match_parent</item>
            <item name="android:layout_height">wrap_content</item>
            <item name="android:layout_marginLeft">12dip</item>
            <item name="android:layout_marginTop">15dip</item>
            <item name="android:textAllCaps">true</item>
            <item name="android:textColor">#999999</item>
            <item name="android:clickable">false</item>
        </style>
    
        <style name="HeaderTextSeparator">
            <item name="android:layout_width">match_parent</item>
            <item name="android:layout_height">1dip</item>
            <item name="android:background">#55CCCCCC</item>
            <item name="android:layout_alignParentLeft">true</item>
            <item name="android:layout_marginLeft">4dip</item>
            <item name="android:layout_marginRight">4dip</item>
            <item name="android:clickable">false</item>
        </style>
    
        <style name="LocationHeaderText">
            <item name="android:layout_width">wrap_content</item>
            <item name="android:layout_height">wrap_content</item>
            <item name="android:layout_marginTop">5dip</item>
            <item name="android:textSize">12sp</item>
            <item name="android:clickable">false</item>
        </style>
    
        <style name="LocationDetailsText">
            <item name="android:layout_width">wrap_content</item>
            <item name="android:layout_height">wrap_content</item>
            <item name="android:textSize">11sp</item>
            <item name="android:textColor">#777777</item>
            <item name="android:clickable">false</item>
        </style>
    
        <style name="LocationTextSeparator">
            <item name="android:layout_width">match_parent</item>
            <item name="android:layout_height">0.1dip</item>
            <item name="android:background">#555555</item>
            <item name="android:layout_alignParentLeft">true</item>
            <item name="android:layout_margin">4dip</item>
            <item name="android:clickable">false</item>
        </style>
    </resources>
    





Here, to keep our code [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we have added a bunch of styles.
Now, we have all the required resources for details activity, which we are going to add next.





17. Create a class `DetailsActivity` and make it extend `SherlockActivity`:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
    public class DetailsActivity extends SherlockActivity {
    
    }
    





18. Override `onCreateMethod()`:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
    ...
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.details);
            setupDefaultValuesFromBundle();
        }
    ...
    





19. Add a `setupDefaultValuesFromBundle()` method:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
    ...
        private void setupDefaultValuesFromBundle() {
            Bundle extras = getIntent().getExtras();
            mDescriptionTextView = (TextView) findViewById(R.id.description);
            mCategorySpinner = (Spinner) findViewById(R.id.category);
            mPreviewImage = (ImageView) findViewById(R.id.previewImage);
            mCurrentTaginfo = extras.getParcelable("taginfo");
    
            mDescriptionTextView.setText(mCurrentTaginfo.getDescription());
    
            String cat = mCurrentTaginfo.getCategory();
            if (cat != null && !cat.equals("")) {
                ArrayAdapter<string> adapter = (ArrayAdapter<string>) mCategorySpinner.getAdapter();
                mCategorySpinner.setSelection(adapter.getPosition(cat));
            }
    
            Uri uri = mCurrentTaginfo.getPictureUri();
            if (uri != null) {
                File file = new File(uri.getPath());
                if (file.exists()) {
                    mSelectedPictureUri = uri;
                    mPreviewImage.setImageBitmap(BitmapFactory.decodeFile(mSelectedPictureUri.getPath()));
                }
            }
    
            ((TextView) findViewById(R.id.detailsAddress1)).setText(mCurrentTaginfo.getAddress1());
            ((TextView) findViewById(R.id.detailsAddress2)).setText(mCurrentTaginfo.getAddress2());
            ((TextView) findViewById(R.id.detailsLatitude)).setText(String.valueOf(mCurrentTaginfo.getLatitude()));
            ((TextView) findViewById(R.id.detailsLongitude)).setText(String.valueOf(mCurrentTaginfo.getLongitude()));
        }
    ...
    





Here, all we are doing is setting the default values for each UI fields.





Although it is very rare nowadays, some of the Android devices still might not have a 'physical' camera for taking pictures. Our app needs to be aware of that. And so, if a camera is not available, we will hide the button for firing up camera intent. Let’s setup these two buttons – one for brining up the gallery and another for brining up the camera, if present.





20. Add a new method `setupImageButtons()`:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
        ...
        private void setupImageButtons() {
            if (getPackageManager().hasSystemFeature(PackageManager.FEATURE_CAMERA)) {
                ImageButton cameraButton = (ImageButton) findViewById(R.id.cameraButton);
                cameraButton.setVisibility(View.VISIBLE);
                cameraButton.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        takePictureWithCamera();
                    }
                });
            }
            ImageButton galleryButton = (ImageButton) findViewById(R.id.galleryButton);
            galleryButton.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    pickPictureFromGallery();
                }
            });
        }
        ...
    





First we check if we have camera feature -- `getPackageManager().hasSystemFeature(PackageManager.FEATURE_CAMERA))`, if so we set the visibility of the cameraButton to `VISIBLE`, and set the onClickListener. This button is hidden by default (check the `res/layout/details.xml` file).





21. Call `setupImageButtons()` from `onCreate()` after calling `setupDefaultValuesFromBundle()`:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
        ...
        @Override
        public void onCreate(Bundle savedInstanceState) {
            ...
            setupDefaultValuesFromBundle();
            setupImageButtons();
        }
        ...
    





We are not going to write our own gallery app, or a camera app. Instead, we will request the built-in gallery to help us pick one picture for the user, which will be done using an intent of type `Intent.ACTION_PICK`, and listen for the result. This is not only convenient on our side, but different apps might have registered for handling the same type of intent. In which case, users will be able to use app of their choice to select an image. This painless integration of apps is one of the many features that make Android platform different and better than others.





Taking a picture from a camera is no different – we just need to fire an intent of type `MediaStore.ACTION_IMAGE_CAPTURE` and wait for the result. Let’s implement both of these features one by one.





22. Add a new method - `takePictureWithCamera()`:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
        ...
        private static final int CAMERA_REQUEST = 0;
    
        private void takePictureWithCamera() {
            Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
            mSelectedPictureUri = getOutputMediaFile();
            intent.putExtra(MediaStore.EXTRA_OUTPUT, mSelectedPictureUri);
            startActivityForResult(intent, CAMERA_REQUEST);
        }
        ...
    





Here we started an activity with an intent of capturing an image. Later, we will be firing up another activity with an intent of picking a picture. To differentiate between these two different requests when the result is available, we need to pass an int ID; that’s what the `CAMERA_REQUEST` is.





We also need a path and a filename to save the picture captured from camera. For this purpose, we need a helper function that creates a uri to a file.





23. Add a new method `getOutputMediaFile()`:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
        ...
        private static Uri getOutputMediaFile() {
            // get the Tagsnap directory
            File mediaStorageDir = new File(Environment.getExternalStoragePublicDirectory(
                    Environment.DIRECTORY_PICTURES), "Tagsnap");
    
            // Create the storage directory if it does not exist
            if (!mediaStorageDir.exists()) {
                if (!mediaStorageDir.mkdirs()) {
                    return null;
                }
            }
    
            // Create a media file name
            String timeStamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
            File mediaFile = new File(mediaStorageDir.getPath() + File.separator +
                    "IMG_" + timeStamp + ".jpg");
    
            return Uri.fromFile(mediaFile);
        }
        ...
    





Before starting to listen for result from camera intent, we will first add a method for firing 'pick' from gallery intent.





24. Add a new method – `pickPictureFromGallery()`:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
        ...
        private static final int GALLERY_REQUEST = 1;
        private void pickPictureFromGallery() {
            Intent intent = new Intent(Intent.ACTION_PICK);
            intent.setType("image/*");
            startActivityForResult(intent, GALLERY_REQUEST);
        }
        ...
    





It's same deal here. We fired up an intent of type `ACTION_PICK` set the type of file to image type, and start the activity with a request code of 1 -- `GALLERY_REQUEST`.
Now we are ready to listen for results from one the above intents.





25. Override `onActionResult()` method:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
        ...
        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (resultCode == RESULT_OK) {
                if (requestCode == CAMERA_REQUEST) {
                    // set the picture
                    mPreviewImage.setImageBitmap(BitmapFactory.decodeFile(mSelectedPictureUri.getPath()));
                } else if (requestCode == GALLERY_REQUEST) {
                    // parse the picture path with some cursor management
                    Uri image = data.getData();
                    String[] filePathColumn = {MediaStore.Images.Media.DATA};
                    Cursor cursor = getContentResolver().query(image, filePathColumn, null, null, null);
                    cursor.moveToFirst();
                    int columnIndex = cursor.getColumnIndex(filePathColumn[0]);
                    String filePath = cursor.getString(columnIndex);
                    cursor.close();
    
                    mSelectedPictureUri = Uri.fromFile(new File(filePath));
                    // set the picture
                    mPreviewImage.setImageBitmap(BitmapFactory.decodeFile(mSelectedPictureUri.getPath()));
                }
            }
        }
        ...
    





If the result is coming back from the capture intent, then we just set the bitmap for our preview image widget. If the result is coming back from the pick intent, we have to do some extra work to find the correct path to the image that user selected, and set the image. It looks good but we have minor issue with this `onActivityResult()` method.





Notice in the line:





`mPreviewImage.setImageBitmap(BitmapFactory.decodeFile(mSelectedPictureUri.getPath()));`





we are decoding the file from `mSelectedPictureUri` that we had saved before firing up the capture intent. The intent brings up a totally new activity – camera’s capture activity. This means the `DetailsActivity` gets killed, and when it is recreated, the `mSelectedPictureUri` will be no more available. We need to make sure to save this uri, in `onSaveInstanceState()`, and restore it in `onCreate()`. But be careful! If we are returning from the gallery activity then `mSelectedPictureUri` will be null so we have to take care of that too.





26. Override `onSaveInstanceState()` method:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
        ...
        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            outState.putParcelable("camera_output_path", mSelectedPictureUri);
        }
        ...
    





27. At the end of the `onCreate()` method, append:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
        ...
        public void onCreate(Bundle savedInstanceState) {
            ...
            if (savedInstanceState != null) {
                mSelectedPictureUri = savedInstanceState.getParcelable("camera_output_path");
                if(mSelectedPictureUri != null) 
                    mPreviewImage.setImageBitmap(BitmapFactory.decodeFile(mSelectedPictureUri.getPath()));
            }
        }
        ...
    





Except than adding the **DONE** and **DISCARD** menu items, we are almost done here. But first we need to set permission for using the camera in `AndroidManifest.xml` file.





28. In `AndroidManifest.xml` file, add the following permission right after the `<uses-sdk>` element:




    
    
        <uses-permission android:name="android.permission.CAMERA"/>
    





## Allowing user to DONE/DISCARD changes with a custom ActionBar





I strong recommend you to [read Roman Nurik’s post on Google+](https://plus.google.com/u/0/113735310430199015092/posts/R49wVvcDoEW) before adding the _DONE/DISCARD_ menu items to a custom _ActionBar_.





29. Start by adding a new `actionbar_custom_view_done.xml` layout:




    
    
    <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
        style="?android:actionButtonStyle"
        android:id="@+id/actionbar_done"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1">
    
        <TextView style="?android:actionBarTabTextStyle"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:paddingRight="20dp"
            android:drawableLeft="@drawable/ done"
            android:drawablePadding="8dp"
            android:gravity="center_vertical"
            android:text="@string/done" />
    </FrameLayout>
    





This layout has a TextView that will be used as a `DONE` menu button.





30. Add two new string resources to `strings.xml`:




    
    
        ...
        <string name="done">Done</string>
        <string name="discard">Discard</string>
        ...
    





31. Add a new menu resource file, `discard.xml`:




    
    
    <menu xmlns:android="http://schemas.android.com/apk/res/android">
        <item
            android:id="@+id/discard"
            android:title="@string/discard"
            android:icon="@drawable/discard"
            android:showAsAction="never"/>
    </menu>
    





Now, we need to setup this custom action bar, and the discard menuitem in `DetailsActivity`.





32. Overrde `onCreateOptionsMenu()`:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
        ...
        @Override
        public boolean onCreateOptionsMenu(Menu menu) {
            super.onCreateOptionsMenu(menu);
            getSupportMenuInflater().inflate(R.menu.discard, menu);
            return true;
        }
    





33. Override `onOptionsItemSelected()` method:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
        ...
        @Override
        public boolean onOptionsItemSelected(MenuItem item) {
            switch (item.getItemId()) {
                case R.id.discard:
                    setResult(RESULT_CANCELED);
                    finish();
                    return true;
            }
            return super.onOptionsItemSelected(item);
        }
        ...
    





When user selects discard menu item, we need to finish this activity after setting result to `RESULT_CANCELED`. This way any activity waiting for the result of this activity can act accordingly.





The last task remaining is to setup our custom action bar, and listen to done button’s click event.





34. Add a new method `setupActionBar()`:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
    ...
        private void setupActionBar() {
            LayoutInflater inflater = (LayoutInflater) getSupportActionBar().getThemedContext().getSystemService(LAYOUT_INFLATER_SERVICE); 
            final View customActionBarView = inflater.inflate(R.layout.actionbar_custom_view_done, null);
    
            customActionBarView.findViewById(R.id.actionbar_done).setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    Intent data = new Intent();
                    mCurrentTaginfo.setDescription(mDescriptionTextView.getText().toString());
                    mCurrentTaginfo.setCategory(mCategorySpinner.getSelectedItem().toString());
                    mCurrentTaginfo.setPictureUri(mSelectedPictureUri);
                    data.putExtra("taginfo", mCurrentTaginfo);
                    setResult(RESULT_OK, data);
                    finish();
                }
            });
    
            // this is what makes the custom actionbar
            final ActionBar actionBar = getSupportActionBar();
            actionBar.setDisplayOptions(ActionBar.DISPLAY_SHOW_CUSTOM, ActionBar.DISPLAY_SHOW_CUSTOM | ActionBar.DISPLAY_SHOW_HOME | ActionBar.DISPLAY_SHOW_TITLE);
            actionBar.setCustomView(customActionBarView);
        }
    ...
    





When the user selects `DONE` button, we create a new intent, set the description, category, and picture uri for the current taginfo object. We then put this updated taginfo back in the intent, and finish the activity with result set to _OK_.





35. Call the `setupActionBar()` from the `onCreate()` method right after `setContentView(R.layout.details)`:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DetailsActivity.java
    ...
        public void onCreate(Bundle savedInstanceState) {
          ...
          setupActionBar();
          ...
        }
    ...
    





We are done with the `DetailsActivity` but we haven’t yet called it from the `DefaultActivity`.





36. Modify `onAddressAvailable()` method to:




    
    //file: src/main/java/com.ashokgelal.tagsnap/main/DefaultActivity.java
    ...
        @Override
        public void onAddressAvailable(Address address) {
            Intent intent = new Intent(this, DetailsActivity.class);
            TagInfo tagsnap = new TagInfo();
            if (address.getMaxAddressLineIndex() > 0)
                tagsnap.setAddress1(address.getAddressLine(0));
    
            tagsnap.setAddress2(String.format("%s, %s, %s", address.getLocality(), address.getAdminArea(), address.getCountryName()));
            tagsnap.setLatitude(address.getLatitude());
            tagsnap.setLongitude(address.getLongitude());
            intent.putExtra("taginfo", tagsnap);
            startActivityForResult(intent, ADD_DETAILS_REQUEST);
        }
    ...
    





Here we create an explicit intent for firing up the `DetailsActivity`, which depends on an instance of `TagInfo` class. So, we create an instance of `TagInfo` class, put it in the intent, start the `DetailsActivity` for result. We won’t be handling the result coming back from the `DetailsActivity` in this part.





If you try to run the app and select the button for adding details, you will get a `ActivityNotFoundException`. If you read the error message carefully, it tells you what’s wrong – we have only defined our `DetailsActivity` class, but haven’t yet declared about this activity to Android system. Every activities in Android that intent to run at some point, should be declared in `AndroidMainifest.xml` file.





37. In `AndroidManifest.xml`, just before the closing `</application>` tag, add:




    
    
    ...
        <activity
            android:name=".DetailsActivity"
            android:label="@string/details"/>
    ...
    





38. Add a new string resource in strings.xml file:




    
    
    ...
        <string name="details">Details</string>
    ...
    





Run the app and try it. Our app is now able to offer a new activity for adding description, a category, and a picture. We won’t do anything with this information for now. We will save these details to a local _SQLite Database_ in the next part of this series.





You can [follow me on Twitter](https://twitter.com/ashokgelal), or [add me on Google+](https://plus.google.com/102672078908622237427/posts). For help, feedback, comments, and other discussions for this tutorial, please [visit the official forum](http://www.ashokgelal.com/forums/forum/android-development/official-tutorials/writing-a-real-android-app-from-scratch-tagsnap/).





See you in the next part.  






  [frameworkads ad="2"]






 



