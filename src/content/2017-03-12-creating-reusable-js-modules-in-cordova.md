---
category: tutorial
date: 2017-03-12 18:30:00 +1100
description: Requiring javascript components in a Cordova plugin via the use of 'js-module' and 'runs'.
layout: article.html
draft: false
tags: cordova javascript technology
thumb: cordova-white-outline.png
title: Creating a reusable js-module in a Cordova plugin
---

## Overview
Cordova supports the creation of javascript components that can be shared within your plugin and others. This tutorial will
cover how to create a reusable `<js-module/>` and how to `require()` it within other components. This is a purely
javascript based tutorial we will not be covering any native functionality.

If you aren't across the difference between `<runs/>`, `<clobbers/>` and `<merges/>` the
[Cordova Documentation](https://cordova.apache.org/docs/en/latest/plugin_ref/spec.html#js-module) is excellent and I would
advise reading it.

## What you will need
### Dependencies
* [node](https://nodejs.org/en/download/)
* [cordova](https://cordova.apache.org/docs/en/latest/guide/cli/)
* [plugman](https://www.npmjs.com/package/plugman)

## Tutorial

### Creating a simple plugin with js-modules

1. [Plugman](https://www.npmjs.com/package/plugman) is a great way to get a Cordova plugin up and running fast. We are
going to use it to create our scaffolding.

    ```bash
    plugman create --name loader --plugin_id cordova-plugin-loader --plugin_version 0.0.1
    ```

    This will create the directory **loader** with the following folder structure.

    ```
    loader/
    |--plugin.xml
    |--src/
    |--www/
    |  |--loader.js
    ```

2. Create a new javascript file named **ajax.component.js** in a new directory **ajax/** under **www/**.

    ```bash
    mkdir www/ajax; cd www/ajax/; touch ajax.component.js
    ```

3. Now lets add some functionality. Below is a code snippet for a simple AJAX request wrapped in a promise and exported.
Open our newly created **ajax.component.js** in your text editor of choice and paste in the below.

    ```javascript
    module.exports = function(options){
        return new Promise(function(success, failure){
            var xhr = new XMLHttpRequest();

            xhr.open(options.method, options.url);
            xhr.onreadystatechange = function(){
                if(xhr.status >= 200 && xhr.status < 300){
                    success(xhr.response);
                }else{
                    failure(xhr.response);
                }
            };
            if(options.data){
                xhr.send(options.data);
            }
        });
    };
    ```

4. To enable us to use this newly created component we are going to add it to our **plugin.xml** file as a `<js-module>`
and define a `<runs/>` node within it.

    First navigate back to the root of our project.

    ```bash
    cd ../../
    ```
    Then open your **plugin.xml** file and add the following snippet under the `<plugin/>` node.

    ```xml
    <js-module name="ajax" src="www/ajax.component.js">
        <runs />
    </js-module>
    ```

    Your final **plugin.xml** file should look like this.

    ```xml
    <?xml version='1.0' encoding='utf-8'?>
    <plugin id="cordova-plugin-loader" version="0.0.1" xmlns="http://apache.org/cordova/ns/plugins/1.0" xmlns:android="http://schemas.android.com/apk/res/android">
        <name>loader</name>
        <js-module name="loader" src="www/loader.js">
            <clobbers target="cordova.plugins.loader" />
        </js-module>
        <js-module name="ajax" src="www/ajax.component.js">
            <runs />
        </js-module>
    </plugin>
    ```

5. Now open **www/loader.js** and require our new module. The namespace for our new module is the id of the plugin plus
the name parameter we assigned our `<js-module/>`. This will make our new modules name **cordova-plugin-loader.ajax**.
Your updated code should now look like this.

    ```javascript
    var exec = require('cordova/exec');
    var ajax = require('cordova-plugin-loader.ajax');

    exports.coolMethod = function(arg0, success, error) {
        exec(success, error, "loader", "coolMethod", [arg0]);
    };
    ```
