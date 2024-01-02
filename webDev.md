# General Web Development Stuff

## How The Internet Works
* there are three main parts:
	* Front end/Client: looks so pretty with HTML/CSS, talks to back end to update, etc
	* Back end/Server: convert data from the database to html, etc to be passed to the front end
	* Database: where bare, naked data is stored
* your domain is the entry point for the user -- it is actually just a key which allows a user to access data via an IP address -- so www.mysite.com is like an alias for your website's IP address
* after you've purchased a domain and web server, you'll need to go to your registrar and transfer the DNS (domain name server) to your web host
	* this might take a minute -- all DNS's need to be redirected to your site
* security
	* OAUTH: logging into pages with facebook/google
	* authorization
		* restrict ppl from certain parts of the site unless they are the right person
	* authentication
	* how to make a username/password

## Domains
* consider: http://www.example.com
	* 'http://' is the PROTOCOL
	* 'www' is the SUBDOMAIN. unnecessary, but it's a way of partitioning a website
	* '.example.com' is the DOMAIN
	* '.com' is the TLD (top level domain)
		* TLDs are increasingly varied these days. they all work just as well.
* buying a domain
	* domain registrars are ways of buying domains
	* godaddy, namecheap
	* whois: public registry of who owns what

## Servers
* also called 'web host'
* shared servers are just fine for a personal website that's small, simple and low-traffic
* host examples:
	* mediatemple
	* dreamhost
	* webhostingforstudents
* uploading your website to the server:
	* use SFTP (secure file transfer protocol using ssh)
* there will be several pre-loaded dirs within your server
	* www is where your site will live
	* within www there's a cgi-bin folder that you can delete (it's for storing cgi scripts -- previously the only place to store executables. it's now a relic)


# Personal (Non-Energy) Web Hosting Options
* your website may consist of static content and applications, so a combination of providers may be the best solution

## Cloud-Based Application Hosting





# The Front End
* its job is to be friendly and pretty
* there are three pillars of the front end: content, presentation, behavior (or `noun`, `adjective` and `verb`)
	* HTML: create structure/content (`noun`)
	```html
		<!doctype html> <!--“hey web browser! this is html” -->
		<html>
			<!-- content goes here -->
		</html>
	```
	* CSS: make it pretty (`adjective`)
		* tools:
			* SASS/LESS: almost exactly the same
		* frameworks:
			* bootstrap/foundation: lets you write a responsive web app with very little effort
			* bootstrap/foundation are both responsive
	* Javascript: add behaviors; interplay between design and back end (`verb`)
		* Frameworks: allow html pages to behave more like applications
			* Backbone.js
			* angular.js (google)
		* bonus points: learn unit testing (Jasmine/Karma) to test your own code
		* jquery/ajax: use web services (jquery is a js library that wraps AJAX functionality amongst many other things)
	* flash used to be useful but is dying out
* more front end build tools:
	* using build tools allows you to minify, transpile and build
	* runs on command line. compresses all js files and spit them out and reload your browser as you save your SASS file
	* grunt: most popular build tool
	* gulp: the new guy, easier to use
	* bower: package management (rather than sending jquery files)
	* requireJS:
	* browserfly: learn this.


# Back End/Servers:
* there are two aspects to the server: the script that serves up code and the physical hardware that it runs on
* there are different types of server services you can pay for/work with
	* CMS: content management system, written with PHP
		* Drupal
		* wordpress
	* app hosting
	* static hosting
	* the kind where you're paying for a 'blank' computer that you totally 'own' and do whatever you want with
* you can use pretty much any language on a server. some common server platforms are:
	* Ruby on Rails: ruby
	* Django: python
	* Express: Node.js
	* PHP: meh. not used for large sites
	* CGI, JSP, ASP, Node.js, Apache, Nginx, MySQL, PostgreSQL, .net(C#)
* caching
	* Squid, Varnish, Memcached
* DevOps: server management (huge sites, i have 20 servers i need to manage)
* databases:
	* MySql
	* MongoDB
	* Redis
* stuff Stephen says you should know about the back end:
	* nodeJS
	* restful interface (graphQL alternative to rest)
	* something that generates sql
	* ORM (optional)
	* def check out node migrations
	* avoid sql injection!
	* yeng says centOS redhat is best server OS


# Backend Services

## API: Application Programming Interface
* sets of requirements that govern how one app talks to another
	* they share some of the program's code
	* defines what you can get and how you can get it
	* ex: embedded google maps, weather, embedded fb content, social logins, etc
* some are free, some are paid
* some can be accessed directly using JS/AJAX so you don't need any server-side programming
* you can use JS, ruby, python or php to talk to third-party APIs


### Restful APIs
* what is REST?
	* Representational State Transfer via HTTP transfer protocols
		* GET, POST, PUT, etc
	* by its nature, the internet is stateless -- every time you make a request to a website, it's like you're meeting it for the very first time
	* we're not passing the resources, we're passing representations of them
	* rest is an architectural style, as opposed to SOAP
* six constraints on REST architecture style:
	1). to be restful, an API must be cacheable
	2). there must be a uniform interface
	3). we use HTTP spec with URIs as resource names and the HTTP verbs as actions we're going to take on those resources
	4). the system is layered
	5). a restful API must provide further code on demand, so it must be stateless and be able to extend logic
	6). uniform interface between the client and the server
* REST supports MVC features like controllers, routing, filter, action results, IOC container, model binders, DI

### REST API Best Practices
* use nouns, not verbs
* [how to name your REST endpoints](https://restfulapi.net/resource-naming/)
* *do not put api keys in query parameters* -- use headers instead. people tend to share urls, and if you put your api key in the url, you're sharing your api key with the world


## WCF Services
* WCF (Windows Communication Foundation) is used for developing SOAP-based services
* uses XML schemas rather than HTTP
* testing is a little trickier
	* use WcfTestClient: a SOAP xml web-based service
	* or you can use Postman apparently

## Rest Vs Soap
* SOAP: XML schemas vs REST's HTTP
* SOAP is ideal for message queues, duplex communication, generally regarded as being more reliable and more secure
* since it uses HTTP, REST can interface with a wide variety of clients including browsers, mobile phones, tablets, etc
* REST gets all kinds of fancy stuff like MVC, routing etc
* REST can leverage caching, URLS, headers, content formats
* WFC can be a bit slower b/c of XML
* REST can use many formats including XML and is preferred for lightweight services
* [rest vs wcf](https://rapidapi.com/blog/wcf-vs-web-api/)
	```
		When you want to create a service that can use expedite transport channels when available such as Named Pipes, TCP, and even UDP, WCF is the ideal choice. Web API can be used when you want to expose an expansive range of clients including, mobile phones, iPhone, tablets, and browsers.
	```

## WCF Hosting
* WCFs can be served via IIS, be self-hosted, or they can be managed through a Windows service
* [read more about WCF hosting](https://docs.microsoft.com/en-us/dotnet/framework/wcf/hosting-services?redirectedfrom=MSDN)

### IIS
* integrated with ASP.NET and uses the offered technology, such as process recycling, idle shut down, process health monitoring, message-based activation
* preferred for services that must be highly available and highly scaleable
* IIS services can only use HTTP transport (IIS 6 and below)

### Self-Hosted
* requires the least amount of infrastructure to deploy
* the WCF service is hosted inside a console application
* this is a useful way to host a service in the development stages

### Managed Windows Service
* long-running WCF services can be hosted outside of IIS in a Windows Service
* managed by the OS

### Windows Process Activation Service (WAS)
* new process activation mechanism
* retains familiar IIS features (app pools, health monitoring, recycling) but removes dependency on HTTP


# Optimization
* chrome developer tools:
	* command+opt+i will pull up chrome dev tools
	* displays the DOM
	* displays any errors while executing your page
	* element inspection
	* you can even alter code within the developer pane to explore changes to your website, but if you refresh, the page (and the original code) will be reloaded
	* by clicking on the 'network' pane you can see how long it took for each element to load
* common problems:
	* syntax: missing semicolon, parens, etc
	* confusing classes and IDs between html and css files
	* selector specificity:
		* do i want to select `.menu a` or just `.menu`?
	* css formattion
		* W3.org/TR/css2/visualformattingmodel

## Javascript Development Environment
* react
	* JS library for creating dynamic visual components
* gulp
	* toolkit for automating time-consuming, repetitive tasks:
		* CSS preprocessing
		* JS transpiling
		* minification
		* live reloading
	* installation:
		* install globally in your development environment
		* for each project, install gulp and gulp utils into dev dependencies
	* getting started:
		* all activity is monitored from one gulpfile.js which tells gulp all
		  its tasks, what they are and when to do them
* node.js
	* open source, cross platform environment for developing server-side applications
* sass/LESS
	* CSS language extension
* bower
	* package manager
* webpack
* AJAX
	* client-side script that communicates with the server
	* exchanges data and updates webpage content without reloading the entire page


## Running A Local Php Server
* instead of MAMP you can moderate Apache and php yourself
* start, stop and restart apache from the command line:
	* apachectl start, stop, restart, graceful
* the computer comes with a shitty version of php so download a fresh one with homebrew
* php.ini is located in root: /etc/php.ini
* run a local php server by calling the following command from your directory:
		`$ php -S localhost:8000`
	* then navigate to 'http://localhost:8000/' in your browser
	* managing php processes:
		* to pause use ctrl+c
		* to kill use ctrl+z
		* to view processes use $jobs
			* this is an alias of ps that's a litte more manageable
			* to kill a job from jobs use $kill %1 %2 %3 to kill jobs
			  1,2 and 3.
			* use $kill -9 %1 when you are real, real serious
* running postgres:
	* we created directory ~/bin/
	* then we created a file called startpostgres which contains:
		`postgres -D /usr/local/var/postgres`
	* in `.bash_rc` we added:
		`export PATH="~/bin:/usr/local/bin":$PATH`
	* these together allow us to type 'startpostgres' from any directory in bash and the postgres command will be called


## Frameworks And Libraries And Servers -- Oh My!
* **angularJS**
	* a front-end framework which allows webpages to behave more like applications and communicates asynchronously with the server
	* using the angular resource factory, you create an abstraction of the server side RESTful API to make server side calls
	* lets you use html as your template language
	* angular manages all the ajax and dom manipulation glue code within a well-defined structure
	* angular teaches the browser new syntax through a construct we call directives:
	* the CRUD application (Create, Read, Update and Delete)
		* data binding
		* DOM control structures for showing, hiding and repeating DOM fragments
		* support for forms and form validation
		* attaching new behavior to DOM elements, like DOM event handling
		* grouping of html elements into reusable components
		* routing
		* deep linking
		* dependency injection
		* testability: unit testing, end-to-end testing, mocks and test harnesses
* the server, in turn, is set up with REST services to respond to these calls
	* for the server-side, you could use a nodeJS framework such as **ExpressJS**
* however you implement your website, making sure that there is a clear distinction between the client and server will make it easier to test and maintain


## Tools
* **Linter**: analyzes code for potential errors
* **Beautifier**: restructures the source code to make indentations and whitespace consistent throughout

# Responsive Web Design
* there are a wide array of screen sizes and devices that access web content
* "mobile-first" approach: it's easier to take something simple and then make it more complex than to scale a big project down
* fluid layout: a page layout flexes and rearranges depending on the dimensions of the page.
* media query
* RESPONSIVE DESIGN is an approach which is addressed using 3 different techniques:
	1) create fluid images (max-width: 100%;)
	2) create a fluid grid: arrange elements using relations and percentages rather than pixels for scaleability
		* a relative-sized object will scale in relation to its parent object, which is at the top-most level, the browser
	3) media queries: allow you to change page elements based on the width of the page
* responsive design may be written inside an additional css document included in the html document right below the primary css inclusion
	* the css inclusions in the head of the html document would go in the following order:
		* normalize.css: create a baseline
		* any font inclusions referred to by main.css
		* main.css
		* responsive.css
		* viewport metatag
	* responsive follows main because it will build on main in response to different cases.

## Media Queries
* before writing queries, determine what the breakpoints for your design are. that is, at what point does this not look good anymore? at what point should the layout change?
* css rules: writing rules containing code that is executed only if the specified condition is true
* syntax and arrangement is as follows:
```css
	/* RECALL THAT THE MAIN.CSS CONTAINS STYLE FOR MOBILE;
		HERE IS STYLE FOR SCREENS LARGER THAN MOBILE

		FOR TABLETS LARGER THAN A MOBILE:
		apply the following code for screens where (width > 480px) == true */
	@media screen and (min-width: 480px) {
		body{
			background: teal;
		}
	}

	/* FOR SMALL DESKTOPS AND UP: */
	@media screen and (min-width: 660px) {
		body{
			background: lime;
		}
	}
```

* the order is important: it follows the rules of cascading style, where the last statement that is true is the one that is executed.

## Choosing Breakpoints And Design:
* mobiles are designed for narrow vertical scrolling
* desktops are more horizontal, and generally all the goodies should fit on the main page.
* viewport metatag:
	* allows for control of mobile pages so that instead of panning and zooming around the page, the page will be locked to the size of the browser window
	* must be added to each html doc
	* example:
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
		* this means essentially that the viewport should be set to the scale of the device's screen width.


## Website Testing
* you can test many characteristics using screenshots of the site and basic image editing software
	* make a screenshot grayscale to make sure everything has sufficient contrast to be legible
	* turn a screenshot 180* to check for composition -- does it look too heavy in one area and empty in another?
	* blur the screenshot to focus on saturation & color balance, and with a lower level of blur, check for text legibility
* check code:
	* w3c validation services can be used to make sure the html follows standards: validator.w3.org
	* for css code: jigsaw.w3.org/css-validator
* real life testing:
	* nothing beats just seeing what it looks like on different devices
	* browserstack.com also provides screenshots of your code -- for a price

## Data
* choosing a database construct: relational or noSql?
* **relational databases**
	* store data in tables as rows and columns
	* tables are joined across multiple relations using primary keys and foreign keys
	* **ACID** or Atomicity, Consistency, Isolation and Durability are key features of relational databases
* **distributed relational databases**
	* relational databases can be distributed, making them horizontally scalable
	* in this case, if a table has 100 records total, there might be 10 records each on 10 notes
	* this process of dividing the data across nodes is called **partitioning** or **sharding**
* **replicated relational databases**
	* database replication is used for load balancing purposes and addressing network latency caused by geographic distance
	* data is replicated on all instances and is synced up via some mechanism
* **NoSQL database**
	* do not support transactional processing -- hence it is not ACIDic

