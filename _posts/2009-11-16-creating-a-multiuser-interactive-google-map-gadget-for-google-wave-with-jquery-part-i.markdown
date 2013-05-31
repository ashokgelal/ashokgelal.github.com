---
author: admin
comments: false
date: 2009-11-16 00:01:15+00:00
layout: post
slug: creating-a-multiuser-interactive-google-map-gadget-for-google-wave-with-jquery-part-i
title: Creating a multiuser interactive Google Map gadget for Google Wave with JQuery
  - Part I
wordpress_id: 269
categories:
- google
- google wave
- programming
tags:
- extension
- gadget
- google
- google wave
- programming
- robot
- script
- wave
---

We just released the beta version of [tweat.org](http://tweat.org/) - a site which maps the best meals in your town. We (by we mean me and the awesome [Trent Cutler](http://trentcutler.com/)) decided that we also need a Tweat extension for Google Wave. It might be very useful for users to find out the best restaurant to go for their next lunch - from right within Google Wave! [I'm already started working on it](http://www.ashokgelal.com/2009/11/getting-ready-for-a-google-wave-extension/), and you should hear about an awesome Google Wave extension pretty soon ;-)





After using Google Wave for about 10 days, I've absolutely fallen in love with it. It's not only fun, but is a very useful collaboration tool. I honestly believe that it will change the way we communicate. One of the most powerful features of Google Wave is its support for extensions. In this post I will show how to create a simple multi-user interactive map gadget. Read on if you are interesting in developing a Google Wave gadget. If you only jumped in to this site looking for a Google Wave invitation, [this might help you out to get one](http://blog.tweat.org/2009/11/tweat-org-is-giving-away-google-wave-invites/). If you have a Google Wave account and want to try this gadget, here is the link to the gadget: [http://wave.tweat.org/gwave/tut/tut.xml](http://wave.tweat.org/gwave/tut/tut.xml)





[ad]





### <!-- more -->
** What really are we going to create?**





A Google map which contains a marker showing the current location. Any one of the participants can change the current location of the map. The 'change' will be reflected on the map of all the participants. There is already a full featured Google Wave gadget called Mappy, and you can even read its source code. This post is just a beginner's guide and also the full featured Mappy is advance and pretty hard to understand.
This is just the first part of a long series of tutorials on Goolge Wave gadget. In other parts we will try to integrate geo-location and other features similar to the one we have in [tweat.org](http://tweat.org/eatlocal/). If you want to see any other features, let me know in the comments.





Though developing a gadget is very easy and intuitive, it is not so easy to test the gadget from within your local server. Unless you have access to Google Wave Sandbox, which I unfortunately don't have, it is going to be a bit difficult. Nevertheless, it is very fun and I would say very good investment to invest some time and efforts in learning how to develop a Google Wave gadget. Before you open up your text editor and start acting like a code monkey, there are few things you need:






    
  1. Obviously, a Google Wave account. If you have someone else that can be with you while you are testing, that would be very convenient! For those of you who don't have any Google Wave account or if you need another one - [this might help you out](http://blog.tweat.org/2009/11/tweat-org-is-giving-away-google-wave-invites/).

    
  2. Get a Google AJAX Libraries api key: [http://code.google.com/apis/maps/signup.html](http://code.google.com/apis/maps/signup.html). We will be using Google Map, and JQuery hosted at Google CDN.

    
  3. After you are ready to test your gadget, you need to host your files in a publicly accessible place. If you don't have one and don't mind sharing your gadget making it an open-source, you can try Google's own Gadgets API Developer Tools [http://code.google.com/apis/gadgets/docs/tools.html#Host](http://code.google.com/apis/gadgets/docs/tools.html#Host) which allows you to host your gadget for free!





That's all we need for now.





### Getting Started:





Fireup your favorite text editor and start with the following:





[html]
&lt;body&gt;
        &lt;div id=&quot;map_canvas&quot; style=&quot;height: 350px; width:500px&quot;&gt;&lt;/div&gt;
        &lt;input type=&quot;text&quot; name=&quot;address&quot; /&gt;
        &lt;input type=&quot;submit&quot; name=&quot;submit&quot; value=&quot;submit&quot; /&gt;
&lt;/body&gt;
[/html]





We just created a 350x500px div to hold our map. We also created an input box to type in the address, and a submit button to geocode the address and show on the map. Let's move into the main part, the javascript part. Add the following codes just before the





[javascript]
&lt;head&gt;
     &lt;script src=&quot;http://www.google.com/jsapi?key=&lt;PUT YOUR JS API HERE&gt;&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;
     &lt;script&gt;
       google.load(&quot;maps&quot;, &quot;2&quot;);
       google.load(&quot;jquery&quot;, &quot;1.3&quot;);
     &lt;/script&gt;
&lt;/head&gt;
[/javascript]





What are we doing here? Not much except than just loading Google Maps (v2) and Jquery (v1.3) library from Google CDN.





Now the fun part, add the following lines just before the closing `__` tag. Don't worry if you don't understand even a single line, we will go through each line and will see what's going on.





[javascript]
&lt;script type=&quot;text/javascript&quot;&gt;
 var map;
 var geocoder;
 var marker;
 $(function(){
      map = new GMap2(document.getElementById(&quot;map_canvas&quot;));
      map.setUIToDefault();
      geocoder = new GClientGeocoder();
      map.getInfoWindow();




    
    <code>  marker = new GMarker(new GLatLng(0,0));
      map.addOverlay(marker);
      map.setCenter(marker.getLatLng(),13);
    
      $('input[name=submit]').bind('click', function(){
           address = $('input[name=address]').val();
           wave.getState().submitDelta({
                'address':address
           });
           return false;
      });
    </code>





});





function reload(){
      address = wave.getState().get('address');
      geocoder.getLatLng(address, function(latlng){
           if(!latlng){
               alert(&quot;Cannot determine the address&quot;);
           }else{
              marker.setLatLng(latlng);
              map.setCenter(latlng, 13);
           }
     })
 }





function init() {
      if (wave &amp;&amp; wave.isInWaveContainer()) {
           wave.setStateCallback(reload);
      }
 }





gadgets.util.registerOnLoadHandler(init);
[/javascript]





[ad]





### Details:




    
    <span style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; line-height: 19px; white-space: normal; font-size: 13px;">Let's talk about the function <code><em>init</em> </code>and the last line:</span>





[javascript]gadgets.util.registerOnLoadHandler(init) [/javascript]





The line says that whenever the gadget is loaded, call the `_init _`method. The name of the function can be anything, it doesn't have to be `_init_`. `_init _`just makes it clear, and intuitive that this function will be called when the gadget will be _initialized_. Now, inside the `_init _`function, we check that the wave has been defined and the wave is in the wave container (which is the job of Google Wave to create and initialize wave object you don't have to worry about this). If it is then whenever there is a change in wave's state, it calls the global function `_reload_`. If you are smart enough, you might have already guessed that the `_reload _`will be the main function handling all of our user interactions.





On the top, we started by declaring three global variables:
`_map _`-> for holding reference to the map that we are going to add
`_geocoder _`-> to convert the given address into latitude/longitude value
`_marker _`-> for holding reference to the marker which will represent the given address on the map





Now, we want to add map to the div _ONLY _after dom is ready for which we will use the wonderful JQuery:





[javascript]
$(function(){
[/javascript]





Now, we will create our map, attach the map to our div, set the map UI to default. We will create a geocoder object, and a marker:





[javascript]
      map = new GMap2(document.getElementById(&quot;map_canvas&quot;));
      map.setUIToDefault();
      geocoder = new GClientGeocoder();
      map.getInfoWindow();
      marker = new GMarker(new GLatLng(0,0));
      map.addOverlay(marker);
      map.setCenter(marker.getLatLng(),13);
[/javascript]





Please notice that the the marker's initial position is at 0,0 and also the initial zoom is 13





Now, we will bind the click action on the _submit _button...





[javascript]
$('input[name=submit]').bind('click', function(){
[/javascript]





...and get the current address from the _input _box:





[javascript]
address = $('input[name=address]').val();
[/javascript]





Now, it is time to make our map react with the submit button's click action. For this we will pass a key/value pair (called map in JavaScript) to wave which will notify our `_reload_ `function (we will come to this function in a few seconds):





[javascript]
wave.getState().submitDelta({
'address':address
});
return false;
[/javascript]





These two lines are really important. We are passing the value from the `_input _`box held in `_address _`variable to wave's current state.





Now, we will write a global function reload which will be called every time one of the participants clicks on the `_submit _`button to set a new address.
We will first grab the value of address from the map that we passed in line 17-18:





[javascript]
address = wave.getState().get('address');
[/javascript]





We will now use Google's geocoding service to get the equivalent latitude/longitude value for this address:





[javascript]
      geocoder.getLatLng(address, function(latlng){
           if(!latlng){
               alert(&quot;Cannot determine the address&quot;);
           }else{
              marker.setLatLng(latlng);
              map.setCenter(latlng, 13);
           }
     })
[/javascript]





If the geocoder returns a latlng value we will set the marker, and center our map to that latlng otherwise we will alert the participants that the address cannot be determined.





That's all for our application part. Now, it is time to wrap up our code in a format recognized by Google Wave. Include following lines on top of the file (before `__`):





[xml]
&lt;Module&gt;
     &lt;ModulePrefs title=&quot;My first Google Wave Gadget&quot; height=&quot;450&quot;&gt;
          &lt;Require feature=&quot;wave&quot;/&gt;
     &lt;/ModulePrefs&gt;
     &lt;Content type=&quot;html&quot;&gt;
         &lt;![CDATA[
[/xml]





`_ModulePrefs _`is the place where you can set all the settings for your gadget such as height, width, title, icon, thumbnail etc. Among other, height is important and you might have to do a little bit of hit-and-trial to get to the height which won't cover a part of your gadget inside a wave. For more information about available settings, [please visit official Google Wave documentation](http://code.google.com/apis/gadgets/docs/basic.html#Userprefs).





Finally, finish off the code by closing the xml tags. At the end of your file, right after </body> tag, include:





[xml]
          ]]&gt;
     &lt;/Content&gt;
&lt;/Module&gt;
[/xml]





### Download the source:





[ad]





[Download the source file from here](http://dl.dropbox.com/u/83257/tut.xml) to see, and be sure that it matches everything with yours.





### What next?





As I've already talked about this earlier, this is just the first part, I will definitely come up with other parts to make this gadget more useful. I do hope that you guys will also share your knowledge, and experience with me, and other readers. I would definitely welcome any feedback and constructive criticism. Keep coming back as the next extension we will be developing, after we finish this gadget, will be a Google Wave Robot!!! For now start a new wave with one of your friends so that you can test out your first ever Google Wave gadget. If you don't find anyone, 'buzz' me at ashokgelal[at]googlewave[dot]com. If I'm online and not busy, I will definitely help you out. In the mean time, don't forget to follow me at Twitter [@ashokgelal](http://twitter.com/ashokgelal) and also tweatorg [@tweatorg](http://twitter.com/tweatorg).





Happy Waving!



