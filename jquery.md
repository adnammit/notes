# JQUERY

## JQUERY VS JAVASCRIPT
* jQuery and JS are sort of the same thing: jquery is a javascript librar
	-- a collection of js code
* gives you a library of tags
* $ can be used as shorthand for 'jQuery' within code blocks
* jQuery supports method chaining
	$(".warning").hide().show("slow");
* jQuery can be used to get, manipulate dimensions, classes, offset etc
* documentation: api.jquery.com
* there are more escape characters for js than just \
		- because we're mucking around with html, it might be good to use
			escape characters for brackets: <p></p> etc

## DOM: THE DOCUMENT OBJECT MODEL
* when a browser interprets an .html document, it reads the tags in a tree-like structure: the html is the root, which gives rise to head and body nodes; body may have h1 and paragraph nodes, etc
* javascript interrogates the process of interpreting and creating the tree structure
* javascript (and more efficiently, jQuery) can be used to smooth over the differences between browsers and make sure that the DOM is formatted the same way across browsers
* examining the DOM within console:
		- 'document' is the root of all other elements
		- typing in document.head.children will display the children of head, which is a child of document
* DOM manipulation methods:
		- add/remove html elements
		- update/read html element attributes
		- transform (hide, show, etc) of elements
* traversal of the DOM
		- moving from a parent to its children
		- moving from a child to its parent
		- moving from an element to its sibling
* event methods
		- keyboard or mouse events
* effects: manipulation over time.

## PUTTING JQUERY IN A PROJECT
* we'll need to include the jquery code
		- we can download it
		- or we can access it with a CDN (content delivery network)
	* the CDN is globally available
	* if it's already been downloaded from the domain before, it will
		be cached and not downloaded again
	* we can access it by using <script src="{the link we find on the
		website}"> in our html head
		- advantage to using a CDN: many users will already have it cached
			resulting in a faster load time
* the .js file we write is also included in the header
* we'll be writing code in the .js file in the form of functions and then
	calling them in accordance with certain events
* if our script tags are in the header, we'll need to execute our code
	when the document is ready
* if our script tags are at the end of the body, we know that the document
	is already ready (the DOM is fully articulated) and we just need to
	write our code, without any functions even.
* putting script tags at the bottom of the page is a best practice because
	it improves the user's experience -- it allows everything else to load
	(more quickly) before the js so the user sees the page and then gets
	the visual experience

## THE FOUR P'S OF PROBLEM SOLVING:
* preparation:
* planning
* perform
* perfect

## USING JQUERY METHODS
* jquery objects have myriad functions for getting and setting css/html
	values, appending tags and content to the page, hiding, animating and
	showing elements, and traversing the DOM
* many functions are getters without args, and setters with args
* some functions interrupt or prevent default behaviours (for example,
	instead of going to the link, maybe we want to show the image in an
	overlay

## LIGHTBOX PROJECT
* overriding default browser behaviours
* the title attribute would be a better candidate for 'caption' than 'alt'

## USING REFERENCES TO FUNCTIONS:
* in jquery sometimes a function (such as an event function) might need a reference to a function rather than calling and using the return value immediately. the reference is passed as focus(eventFunction) with no parens to make it a reference:
	// pass references to passwordEvent (rather than the return value)
	// to focus so focus can call passwordEvent when it needs it
	$("#password").focus(passwordEvent).keyup(passwordEvent);



## JQUERY PLUGINS
* plugins allow you to easily add complex features (like image galleries, calendars, pop-ups, slider menus, etc)
* examples:
		- vide: add video as a background to any webpage
		- jquery adaptive modal: create animated links to display data
		- lightbox: image display
		- jqueryui.com: countless options
* places to find good plugins
		- sitepoint.com
		- unheap.com
* how to choose a plugin:
		- keep it simple
		- keep it fast
		- get a plugin that does what you need it to do
		- look for plugins with good documentation
		- make sure it's current/updated
		- responsive-friendly (app and desktop compatible)
* plugins usually include:
		- css file (usually)
		- a js file (always)
		- images (usually)
		- may include a /dist folder and a /src folder
	* the /dist files are the ones you want
	* may be divided into /css and /js subdirectories
	* you can move the files from /css and /js into a master /myPlugin
		dir if you want to, as long as you change your file references
		accordingly. this will make it cleaner
	* just remember to keep all plugin files together so it can be
		more easily modified or deleted.
		- min files: minimized files, compressed and w/ whitespace removed
		- for development you'll want to use the full files that you can read,
			but for your live webpage, you may want to use the min files.
* using a plugin:
		- include the css file, before your own site's css file
		- attach jQuery script or CDN reference first, followed by the plugin
			script and then your script which will build off of the jQuery and
			plugin code
		- structure your html in the specified way
		- add your javascript code (either embedded or external file)
		- select an element on the page using jquery ($ selector)
		- lastly, call the plugin function using the selection
	// here is the selection and function call for the animsition PI:
			<script>
		$(".animsition").animsition();
			</script>
		- you have the added ability to customize plugin functionality by
			passing in a js object literal as an argument to the plugin function
			call
	* specifically, this consists of properties and values which will
		modify the settings
* plugin events
		- events: mouseover, click, scrolling, clicking on a link, focusing an
			element
		- responses: how does the app respond to a user input?
		- some events are user-triggered, some are inherent
	* ex: for sticky: when the user scrolls down and the page reaches
		a certain point, the sticky menu starts
* carousel plugin: slick
		- slideshow of highlights of various content throughout the site
		- consists of an outer carousel div and one div for each slide which
			contains the html for each


