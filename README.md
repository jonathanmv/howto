howto
=====

Just collecting some howtos on meteor, cordova etc.

#Cordova 3.x and Meteor
Ok some stuff to be aware of:


##Appcache
Appcache in 2.x android is not working above approximate 3.5-3.7mb - this can be fixed
In the main java file add
```java
        import android.webkit.WebSettings;
        
        ...
        
        super.onCreate(savedInstanceState);
        // Set by <content src="index.html" /> in config.xml
        super.loadUrl(Config.getStartUrl());
        //super.loadUrl("file:///android_asset/www/index.html")
        ....
        // Add this to activate 8mb appcache
        super.appView.getSettings().setDomStorageEnabled(true);
        super.appView.getSettings().setAppCacheMaxSize(1024*1024*8);
        super.appView.getSettings().setAppCachePath("/data/data/dk.gi2.sitdrift/cache");
        super.appView.getSettings().setAllowFileAccess(true);
        super.appView.getSettings().setAppCacheEnabled(true);
        super.appView.getSettings().setCacheMode(WebSettings.LOAD_DEFAULT);
```

##Fullscreen
In cordova 3.x theres a flag in the config.xml
```xml
  <preference name="fullscreen" value="true" />
```
But... The version I'm using I have to activate this in the IOS config too:
/project/platforms/ios/Hello/*.plist
Add this flag:
```xml
    <key>UIStatusBarHidden</key>
    <true/>
```

In the html use the normal meta settings:
```html
  <meta name="viewport" content="width=device-width, height=device-height, initial-scale=1, maximum-scale=1, user-scalable=no"/>
```

And remember to set some styling on the html:
```css
   * {
      -webkit-box-sizing: border-box;
      -moz-box-sizing: border-box;
      box-sizing: border-box;
    }

    html {
      -ms-touch-action: none;
      height: 100%;
      background-color: black;
    }

    html, body, iframe {
      overflow: hidden;
    }

    body {
      height: 100%;
      padding: 0;
      margin: 0;
      border: 0;
      background-color: black;
    }

    /* I'm using an iframe for my meteor as a shell - this way I can fallback if no connection on load */
    /* I've built a simple interface for meteor to communicate with cordova through the iframe */
    iframe {
      background-color: black;
      border: none;
      height: 100%;
      width: 100%;
    }
```
