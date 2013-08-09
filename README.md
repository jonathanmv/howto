howto
=====

Just collecting some howtos on meteor, cordova etc.

#Cordova 3.x and Meteor
Ok some stuff to be aware of:

##CLI
The new cordova cli is really great - I only use this when writing so no eclipse og xcode have to be open + you only have one code and config to work on (almost)
When making direct changes in the platforms folder these are preserved - this is really nice specially the configs.

In short:

###Install
```bash
$ npm install cordova -g
```

###Create
```bash
$ cordova create hello com.domain.hello Hello
```

###Add platforms
```bash
$ cd hello
$ cordova platform add android
$ cordova platform add ios
```

###Plugins
This is a really great feature - you can add plugins directly from git
Eg. add the `device`
```bash
$ cordova plugin add https://git-wip-us.apache.org/repos/asf/cordova-plugin-device.git
```

Remove
```bash
$ cordova plugins
[ 'org.apache.cordova.core.device' ]

$ cordova plugin rm org.apache.cordova.core.device
```

*This installs and rigs the plugin for all the platforms, nice*

###Build
Use this step if you have altered some config or platform code
```bash
$ cordova build
```

###Emulate
Again a super cool featur
```bash
$ cordova emulate iphone
```
*This fires up the emulator on ios - might have to set the device in the emulator to iphone + adt / eclipse to configure the default emulated device*

###Run
If you have a device connected you can run the app on the device
```bash
$ cordova run android
```
*You might have to open adt or eclipse to connect the device the first time*

###Logcat
Using android device to run the app you might want to access the console
```bash
$ adb logcat
```
You can also just grap the console.log's from the html
```bash
$ adb logcat | grep "I/Web"
```
*You can grep anything - eg. if you do console.log('DEBUG: ' + myMessage) then grep out the 'DEBUG:' to isolate the log to this output*

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
