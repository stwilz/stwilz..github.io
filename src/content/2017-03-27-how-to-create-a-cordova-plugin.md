---
category: tutorial
date: 2017-03-27 18:30:00 +1100
description: A step by step tutorial to creating a Cordova plugin
layout: article.html
draft: false
tags: cordova javascript technology
thumb: cordova-white-outline.png
title: How to create a Cordova Plugin
---
## Overview
### Surely someone has covered this...
They sure have! However when I first started investigating how to create a basic Cordova plugin I found that there was a
quit a bit of digging involved. By the time I'd created my first plugin I had a hand full of lessons learnt.
This tutorial covers all the steps and information I believe to be important for creating a Cordova plugin.

### What you will need
* terminal
* any kind of text editor ([Atom](https://atom.io/), [Visual Studio Code](https://code.visualstudio.com/)).
* [nodejs](https://nodejs.org/en/download/)
* [cordova](https://cordova.apache.org/#getstarted)
* [plugman](https://github.com/apache/cordova-plugman)

### Just give me the code!
Sure, if you just want to skip right to the code and see how everything works
here you go [git](https://www.github.com/stwilz/cordova-plugin-boilerplate).

## Tutorial
### Folder structure
Below is the proposed folder structure. There is no enforced structure for where you wish to put your javascript and
native code. In fact in my tutorial [third party library] I cover how to import a javascript library that is a node
package. The convention of using **/src** for all native code and **www/** for all javascript is just one
```
plugin
|--plugin.xml
|--www/
|   |--hello-world.js
|--src/
|   |--ios/
|   |   |--hello-world.m
|   |--android/
|   |   |--hello-world.java
```

1. You are going to use [plugman](https://github.com/apache/cordova-plugman) to create our boilerplate files. This will
generate our **plugin.xml**, **www/hello-world.js** and an emptt **src/** directory.

    Also another important thing to note is how to name your Cordova plugin.

    ```bash
    plugman create --name hello-world --plugin_id cordova-hello-world --plugin_version 0.1.0 --plugin_dir ./
    ```
2. Now we should update our Javascript method. Firstly, it is currently named `coolMethod` and secondly, the method should
be converted to a promise as it will make your methods more flexible in the future.
    ```javascript
    var exec = require('cordova/exec');

    exports.helloWorld = function(arg0) {
        return new Promise(function(success, error){
            exec(success, error, 'hello-world', 'helloWorld', [arg0]);
        });
    };
    ```
3. Plugman gives us a simple cli command to to add the Android platform. The following command will generate a
new **www/android/hello-world.java** as well us updating our **plugin.xml** with a `<platform name="android">` node.

    ```bash
    plugman platform add --platform_name android
    ```
    Unfortunately it seems that plugman does not support an argument for setting our method name so we will need to open
    our newly created **www/android/hello-world.java** file and do a quick refactor of the `coolMethod` value to `helloWorld`.
    Our updated **.java** file should look as follows

    ```java
    package cordova-hello-world;

    import org.apache.cordova.CordovaPlugin;
    import org.apache.cordova.CallbackContext;

    import org.json.JSONArray;
    import org.json.JSONException;
    import org.json.JSONObject;

    /**
     * This class echoes a string called from JavaScript.
     */
    public class hello-world extends CordovaPlugin {

        @Override
        public boolean execute(String action, JSONArray args, CallbackContext callbackContext) throws JSONException {
            if (action.equals("helloWorld")) {
                String message = args.getString(0);
                this.helloWorld(message, callbackContext);
                return true;
            }
            return false;
        }

        private void helloWorld(String message, CallbackContext callbackContext) {
            if (message != null && message.length() > 0) {
                callbackContext.success(message);
            } else {
                callbackContext.error("Expected one non-empty string argument.");
            }
        }
    }
    ```
4. Most Cordova plugins you will see typically use Objective-C for their iOS code but don't be fooled!
[Swift is already one of the worlds most popular programming languages](http://www.cultofmac.com/471301/swift-is-already-of-the-worlds-most-popular-programming-languages/),
you can and should be using it! Simon Pricket at Modus Create has a great article on
[writing a Cordova plugin in swift](http://moduscreate.com/writing-a-cordova-plugin-in-swift-3-for-ios/).

    * First create your swift file

        ```bash
        mkdir src/ios; cd src/ios; touch hello-world.swift
        ```
    * Now open the newly created **hello-world.swift** and update it's contents with the following

        ```swift
        @objc(HelloWorld) class HelloWorld : CDVPlugin {
            @objc(helloWold:)

            var pluginResult = CDVPluginResult(
              status: CDVCommandStatus_ERROR
            )

            func helloWorld(command: CDVInvokedUrlCommand) {
                self.commandDelegate!.send(
                    pluginResult,
                    callbackId: command.callbackId
                )
            }
        }
        ```

    * `cd` back to the root directory of your plugin and update the **plugin.xml** file with a your iOS settings

        ```xml
        <platform name="ios">
            <config-file parent="/*" target="config.xml">
                <feature name="hello-world">
                    <param name="ios-package" value="hello-world"/>
                </feature>
            </config-file>
            <source-file src="src/ios/hello-world.swift"/>
        </platform>
        ```
    * And finally we are going to need to update our **plugin.xml** with a [Cordova Dependency](https://cordova.apache.org/docs/en/latest/plugin_ref/spec.html#dependency)
    for the [Swift Support Plugin](https://github.com/akofman/cordova-plugin-add-swift-support).

        ```xml
        <dependency id="cordova-plugin-add-swift-support" version="1.6.1"/>
        ```

5. Now lets install this in a Cordova app to prove all of this works.

    * In your terminal `cd` back to the root of your plugin directory and run the following command to create a Cordova
    helloWorld app with both the iOS and Android platforms.

        ```bash
        cordova create example-app io.example.app HelloWorld; cd example-app; cordova platform add ios --save; cordova platform add android --save
        ```

    * Once you have run the command above your terminal will now be in the **example-app** directory. What we need to do
    next is run a plugman command that will tell our plugin to install in the appropriate platform. If you copy and paste
    the following into your terminal it will install the plugin in the above directory into each platform directory.

        ```bash
        plugman install --platform ios --project ./platforms/ios --plugin  ../../hello-world && plugman install --platform android --project ./platforms/ios --plugin ../../hello-world
        ```

        Also, as you continue to develop your plugin you may want to remove the plugin. You can do this via the `uninstall` cli command like so

        ```
        plugman uninstall --platform ios --project ./example-app/platforms/ios --plugin ../../hello-world && plugman uninstall --platform android --project ./example-app/platforms/ios --plugin ../../hello-world
        ```

    * Run it!
        ```bash
        cordova build
        ```
