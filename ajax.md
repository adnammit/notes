INTRO TO AJAX
o what is AJAX?
    - AJAX is a client-side script that communicates to and from a
      server/database without the need for a postback or a complete page
      refresh
    - method of exchanging data with a server and updating parts of a web
      page without reloading the entire page.
    - "Asynchronous Javascript and XML"
    - allows updating of HTML
    - used in googlemaps, facebook, twitter, etc
    - new content without new pages
    - technically, what we call AJAX is actually an XHR: XML HTTP Request 
      object
o when to use AJAX and when to use a new page?
    - lots of data or a little data?
    - same format or a different format?
    - for the same amount of data, AJAX is faster than loading a new page
      because you don't have the overhead of loading the page structure
      and content all over again.
o how it works:
    - a REQUEST is sent from the client (browser) to the server
    - a RESPONSE is sent from the server to the client
    - ASYNCHRONOUS: Asynchonous requests are made and we don't hang out
      waiting for an answer -- the user can continue browsing and using
      the page while the request is processed
	* multiple requests may be sent out and we don't know which will
	  come back first due to varying times required to process and
	  return the response
    - JAVASCRIPT: Javascript and jQuery are used to generate requests as
      well as interpret the responeses and alter the page content
      accordingly
    - AND XML: xml used to be the preferred markup language used for
      packaging requests and responses -- no longer, but the name stuck
    - watch it in action with the Chrome developer tool:
	* in 'Console' right click on the console window and select 'Log
	  XMLHttpRequests'
	* do some things on the website that generates a request and watch
	  the logs pile up
o how to AJAX step by step:
    1). create an XMLHTTP Request Object
    2). create a callback function which will kick off when the server
        returns a response
    3). open a request with two pieces of information
	a). method the browser uses to send ('get' or 'post')
	b). the url where the request is sent
    4). send the request
o run AJAX locally by using a php server (or whatever scripting server you
  want to)
o limitations of AJAX
    - AJAX has a 'same origin policy": it can only be used to access data
      on the same server
	* pages hosted by the same server can query the server, but a page
	  from another server cannot
    - you cannot switch protocols (ie http->https) even on the same server
    - you cannot switch ports (default is 80 -- switching to another port
      is forbidden
    - switching hosts is not allowed: a page hosted by www.myserver.com
      cannot talk to db.myserver.com
o but we constantly need data from other websites. how to circumvent the
  same-origin policy:
    - using a WEB PROXY: pages can't talk to servers they don't belong to,
      but SERVERS can talk to each other
    - using JSONP: JSON with padding
	* this relies on CROSS DOMAIN LINKS
	* browsers do this in many other ways: you can link to a photo on
	  another website, css, JS, jQuery, etc can all come from other
	  domains
	* this is also how CDNs (Content Delivery Methods) work
	* instead of actually accessing the other server, you're accessin
	  a JS file from the site
	* jQuery makes this really easy to do
    - using CORS: Cross-Origin Resource Sharing
	* requires some set up with the server but allows access from
	  other domains

EVENTS
o Ajax comes with its own set of events
    - for JS, events include mouseclicks, keyboard input, and closing or
      modifying a window
    - for Ajax, events include changes in request/response statuses
    - the most important Ajax event is called OnReadyStateChange
	* this event is triggered whenever there is a change in an Ajax
	  request (opening, sending, receiving a response)
	* our callback function is created to respond to that event

XHR OBJECTS
o create your XHR object with a simple line of code, placed between the
  script tags in the head of your html document
	var xhr = new XMLHTTPRequest();
o each XHR object has a property called readyState
o the readyState property contains an integer value (1-4) which
  corresponds to a state change in the Ajax request
    - for example, #4 means 'hey we got a response back!'
o XHR objects have a property called 'status' that looks for a server
  error, such as 404 (not found)
    - we might use the following conditional statement in our callback
      function to look not only for a response but also to make sure we
      connected to the server in the first place so the browser isn't
      waiting around forever for nothing:
	    if (xhr.readyState === 4 && xhr.status === 200)
	* '200' is the normal, good response.
	* 500 means we connected to the server, but there was some other
	  error
	* you can add other conditionals to take other actions for
	  specific server responses
	* you can use the following block to display an alert for
	  conditions other than 200:
	    } else {
		alert(xhr.statusText);
	    }
o XHR objects also have a property called "responseText". When a response
  is received, this property is set to the value returned from the server
o open method: this function prepares the browser to send a request
    - the open method takes two arguements:
	* the first is the HTTP method that you're going to use
	    o the most common methods are 'GET' (retrieve data) and 'POST'
	      (sending data)
	* the second is the url where the request is going, perhaps a file
	  or a server side program. for learning purposes, it could be a
	  simple html file
o send method: if you're sending data to the server, all you have to do is
  call the function with no arguments. if you're sending data, your send
  function will take the data as an arg


MY FIRST AJAX EXAMPLE
o you'll need a server for it to work, be it a real one, or a local server
o implementation:
    - create a div tag in the html body that will be loaded with the
      content from the server
    - create your XHR object and callback function (in this case, located
      in the head of the html document within script tags
    - you should create a new object for each request
    - creating your callback function
	* when working with multiple requests: if the responses may return
	  in any order, that means the callback functions may run in any
	  order
    - to trigger the callback function, we use a browser event such as
      OnReadyStateChange -- when it equals 4, we've got an answer from the
      server
    - when we get a response, we're going to load it into a div element
      with an id of 'ajax' by using 'innerHTML' which replaces the html
      inside the element with the specified html.
    - use the xhr object's 'open' method to open the request
    - use the xhr object's 'send' method to send it
    - when the request comes back, the readyState will change, and the
      callback function will execute.


  <head>
    <script>
	var xhr = new XMLHttpRequest();
	xhr.onreadystatechange = function () {
	    if (xhr.readyState === 4 && xhr.status === 200){
		document.getElementById('ajax').innerHTML
		    = xhr.responseText;
	    }
	};
        xhr.open('GET', 'sidebar.html');
	function sendAJAX(){
	  xhr.send();
	  document.getElementById('load').style.display = "none";
	};
  </script>
  </head>

  <body>
    <button id="load" onclick="sendAJAX()">Bring it!</button>
    <div id="ajax">
      <!-- AJAX data goes here when code runs! -->
    </div>
  </body>



DYNAMIC INFORMATION: GET AND POST METHODS
o 'GET' is actually the same method used to 'get' the content of your page
  in the first place
o query strings (in the example below, the string is '?lastName=Jones')
  allow the browser to send the server an extra bit of information to
  control which data is accessed and displayed
	http://www.website.com/employees.php?lastName=Jones
    - generally query strings are used to display a single item of data or
      a small subset from a larger body of data
    - in the above example we can see that the page being queried is in
      php rather than html
o query strings can hold multiple name-value pairs
    - a name-value pair is 'lastName=Jones'
	http://www.website.com/employees.php?firstName=Rita&lastName=Jones
o requirements:
    - special characters: &, =, space, ""
	* these symbols all have special meaning in a url so they need to
	  be encoded (turned into a special set of characters) so they're
	  safe and hold their meaning
    - online sites can be used to convert text to safe, encoded text
    - there are also javascript tools that can encode text
o downsides to 'get':
    - the data request is sent in the url, which means that if you need to
      send sensitive information, it shows up in the browser's address bar
      and in the log files
    - there's only so much information you can put into a url. ie has a
      char limit of ~2800 so sending a blog post like this isn't a good
      idea
o POST, on the other hand, sends data separately from the url, in the body
  of the request
    - more secure than GET
    - allows more information to be sent
    - also requires encoding
    - requires a special header: an instruction sent to the server telling
      it what kind of data it should expect
o should i use GET or POST?
    - use GET when requesting information:
	* looking for search results
	* getting photos from a photostream
	* getting html to add to the page
    - use POST when sending information to be stored in the database
	* sending lots of info from a form
	* handling sensitive info like a password, phone #, email, etc


RECEIVING AJAX RESPONSES
o server responses are in the form of simple text
    - whether the resoponse is in HTML, XML or JSON, the browser recieves
      it as plain text to be interpreted by your code
	* the data must be PARSED to be used by the browser in a
	  meaningful way
    - this could be something simple like 'ok, cool' or 'nope'
    - the response could also be very large: say, 50 new tweets or 100
      more items
	* for dealing with data of this magnatude, you should use a
	  structured format, meaning that it is ordered consistantly,
	  has identifiers to indicate what the data is, and is easy for
	  JS to use.
	* common data interchange formats are XML and JSON
	* XML is the X in AJAX
	    o like html, it uses tags
	    o clean, clear, easy for the computer to parse
	    o however, using XML with JS is involved: it requires several
	      steps to parse and extract the data
	* because of these limitations, JSON is a better choice when
	  working with JS
	    o use of arrays, property-value pairs, or JS objects

RECEIVING JSON
o you can use the JSON.parse("foo"); method built into your browser to
  parse the plain text response from the server into workable JS
    - for example, creating an array of JS objects called 'employees':
	    var employees = JSON.parse(xhr.responseText);
