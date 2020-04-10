## NPM

### NVM
* first let's talk about `nvm`, the package manager for your package manager
* download and install nvm  -- then you can install, uninstall and use different versions of npm more flexibly
    ```
        nvm ls/list                 view downloaded versions
        nvm ls-remote               view versions available to download
        nvm install 10.5.0          install a specific version
        nvm install latest          install the latest version
        nvm use 10.5.0              you gotta say "use" if you actually want to use it -- just installing won't work
        nvm alias default 10.5.0    set as default
    ```

* npm (Node Package Manager) is the default package manager for NodeJS
* some npm functionality:
```
    which npm                       display version
    npm search foo                  search for packages like 'foo'
    npm list -g --depth=0           view installed packages (-g for global - omit for local)
    npm cache clean                 clear cache
    npm prune                       remove unneeded packages
    npm install <foo> --save-dev    use when you want this to be available to be downloaded by other developers (say, grunt or gulp)
    npm install <foo> --save        use when you have a dependency which needs to be available for distribution (say, angularjs or express)
```

* should I install globally or locally?
    - if you're installing something that you want to use in your program, using require('whatever'), then install it locally, at the root of your project.
    - if you're installing something that you want to use in your shell, on the command line or something, install it globally, so that its binaries end up in your PATH environment variable.
    - sometimes you will want both. seem excessive? don't worry - the space usage isn't significant
    - you can also symlink to the global install (or `npm link`?) but just installing it in both places is preferable
* notes on installing packages with npm:
    - is npm installing in `..` rather than your `pwd`? you might need to `$ mkdir node_modules` first so npm doesn't get distracted and try to walk up the folder structure
* installing with `--save` or `--save-dev` will install the necessary dependencies for distribution. Forgoing both will strip away those dependencies leaving others clueless to what is needed to run your dang thing, so try to use one or the other


### USING PACKAGE.JSON
* why use `package.json`?
    - using `package.json` allows you to record your dependencies for webpack and babel and install them again with `npm install`
    - you can define a "start" script so you don't even have to type out `webpack-dev-server etc` -- just do `npm start`
* how to use:
    - go to your project directory and type `$ npm init`
    - follow prompts and enter your `src/main.js` or whatever as your entry point
    - add a new key to the file to tell it what to do for `$ npm start`:
        `"start": "node_modules/webpack-dev-server/bin/webpack-dev-server.js"`



## NODEJS
* Node allows you to run applications on the server that are written in javascript
* node is built on google's V8 JS runtime engine and written in C++ so it's fast
* used to write developer tools like Grunt and Gulp

### HOW DOES NODE FIT INTO THE LARGER JS ECOSYSTEM?
* when we use JS in a browser, it's interpreted against the browser's engine and is run against browser-specific APIs like the DOM
    * (yes, the DOM is an API)
* when we're thinking about javascript in a browser we can divide it into two distinct parts:
    - native objects
        * strings, arrays, dates, math
        * these can be used in any JS environment
    - host objects
        * in the browser, these include: window, document, history, xml, httprequest, etc
* when google introduced the open-source v8 js engine, it opened the door for js to do more than just make web pages
* with the off-browser v8 engine, we have the same native objects with a new cast of host objects:
    - http, https, fs (filesystem), url, etc
* the coupling of the v8 engine with apis create the nodejs platform
* aside from being fast, why is nodejs so badass?
    - it's non-blocking -- we can do a bunch of different tasks at once, without blocking other requests
    - other platforms use 'workers' -- multiple instances of the same process which all work on different tasks
    - but nodejs is just one super-efficient worker that does what it can do as soon as it can

### GETTING STARTED
* you can run node code that's saved as a .js file by typing:
    `$ node app.js`
* you can also enter a Read-Evaluate-Print loop (REPL) and play with
  node on the command line:
    ```bash
        $ node
        > 1+2
        3
    ```

* STOP WITH:
    ```
    taskkill /F /IM node.exe
    ```

"server": "babel-node server/index.js",
"server": "nodemon --watch server --exec babel-node -- server/index.js",



### NODEJS OBJECTS
* what can we do with node? let's look at the docs:
    - some host objects of note: http, https, fs
* you may see the .on method come up a lot -- this means that the callback
  is responding to an event
    ```javascript
    req.on('error', function(e) {
    console.log("problem with request: " + e);    
    });
    ```
