---
categories: tutorials
date:   2017-03-19 11:00:00 +1100
description: "With the correct Cordova hooks you can stop adding third party dependencies to your plugin repository."
layout: article.html
published: true
tags: cordova javascript technology npm node
thumb: ""
title:  "Using NPM to manage dependencies in a Cordova Plugin"
---
Recently in my tutorial [Creating reusable js-module's in Cordova](/tutorials/2017/03/13/creating-reusable-js-modules-in-cordova.html)
I explained how to create a `<js-module>` that can be shared across your plugin. But what if we want to create a `<js-module>`
from a third party library? And what if we want to use a dependency manager for that library?

I recently attempted to do just that with [npm](https://www.npmjs.com/) and found that with the implementation of a pre-install hook I was able
to get my node modules to install before the plugin and then reference them within my **plugin.xml**.

## Here's how to do it!
I'm going to continue this tutorial from where

1. We will need to create a **package.json** file to list our node dependencies in. To do so open up your terminal and
type in the following command.
    ```bash
    cd loader; npm init
    ```
    For the sake of this tutorial you can select all of the default options.

2. I've chosen the [Event Emitter 2](https://github.com/asyncly/EventEmitter2) to add to our library. We will combine this
with our ajax component later. To add the dependency to our plugin type the following command into your terminal.
    ```bash
    npm install --save eventemitter2
    ```
3. Next we need to update our **plugin.xml** with the reference to our newly installed library.
    ```xml
    <js-module name="EventEmitter2" src="node_modules/eventemitter2/lib/eventemitter2.js">
        <runs/>
    </js-module>
    ```
4. Since our EventEmitter2 library won't be available in our repository when user's install the plugin we need to add a
Cordova hook that will run our `npm install` before the plugin installs.
    ```bash
    mkdir hooks; cd hooks; touch install_dependencies.js
    ```

5. Now open the newly created file**install_dependencies.js** and copy the following javascript.
    ```javascript
    module.exports = function (context) {
        var shell = context.requireCordovaModule('shelljs');

        shell.cd(context.opts.plugin.dir);
        shell.exec('npm install');
    };
    ```

6. To have the hook execute we need to add a `<hook>` node to our **plugin.xml**. Open up your **plugin.xml** file and
add the following xml.
    ```xml
    <hook type="before_plugin_install" src="hooks/install_dependencies.js" />
    ```

    It's important to note that the **type** value is required to determine the behaviour of the hook. You may see examples
    where the hook is installed in a directory with the name of it's type ie **./hooks/before_install/**, however this
    convention is [deprecated](https://cordova.apache.org/docs/en/latest/guide/appdev/hooks/#via-hooks-directory-deprecated).

7. Finally all we need to do is reference our newly created `<js-module>` in another javascript component. I'm going to
open the **ajax.component.js** we created in our previous project.

    ```javascript
    var eventEmitter = require('cordova-plugin-ajax.eventEmitter');
    ```