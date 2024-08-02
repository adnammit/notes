# Node

## Overview
* node is cross-platform open-source js code that runs on servers
* it uses async event-driven model and is efficient and scalable
* some use cases for Node.js include:
	* serverside template rendering for a SPA
	* data streaming apps: http requests can be used for bufferless transfer between client and server
	* data-intensive real time apps: transmitting large amounts of data to many users simultaneously, e.g. multiplayer games or stock market trackers
	* on-demand apps: ex: uber; rapidly changing data and thousands of concurrent connections happening simultaneously

## Node.js
* Node allows you to run applications on the server that are written in javascript
* node is built on google's V8 JS runtime engine and written in C++ so it's fast
* used to write developer tools like Grunt and Gulp

### How Does Node Fit Into The Larger JS Ecosystem?
* when we use JS in a browser, it's interpreted against the browser's engine and is run against browser-specific APIs like the DOM (*yes, the DOM is an API*)
* when we're thinking about javascript in a browser we can divide it into two distinct parts:
	* native objects
		* strings, arrays, dates, math
		* these can be used in any JS environment
	* host objects
		* in the browser, these include: window, document, history, xml, httprequest, etc
* when google introduced the open-source v8 js engine, it opened the door for js to do more than just make web pages
* with the off-browser v8 engine, we have the same native objects with a new cast of host objects:
	* http, https, fs (filesystem), url, etc
* the coupling of the v8 engine with apis create the Node.js platform
* aside from being fast, why is Node.js so badass?
	* it's non-blocking -- we can do a bunch of different tasks at once, without blocking other requests
	* other platforms use 'workers' -- multiple instances of the same process which all work on different tasks
	* but Node.js is just one super-efficient worker that does what it can do as soon as it can

## Getting Started
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



## Node.js Objects
* what can we do with node? let's look at the docs:
	* some host objects of note: http, https, fs
* you may see the .on method come up a lot -- this means that the callback
  is responding to an event
	```javascript
		req.on('error', function(e) {
		console.log("problem with request: " + e);
		});
	```

## Express
* express is a web app framework for Node.js
* express provides various features that make development fast and easy -- kind of like the asp.net to .net
* can be easily integrated with databases like mongo, redis etc. node by itself lacks support for relational databases
* express is lightweight and easy to implement. use cases include:
	* rest apis
	* static file hosting
	* real time services such as chat applications -- express supports websockets, which is great for data streaming over persistent connections

## Nodemon
* nodemon is a nice companion for express/node apps -- it watches your files and automatically restarts your app when changes are detected
