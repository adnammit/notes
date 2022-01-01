# SCALABILITY AND SYSTEM DESIGN FOR DEVELOPERS

## RESOURCES
* [course at educative.io](https://www.educative.io/path/scalability-system-design)
* [Overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
* [RESTful architecture](https://restfulapi.net/)


## TIERED APPLICATIONS
* what is a **tier**? a tier is a logical, physical separation of components in an application or service
* examples of **components** include database, backend app server, UI, messaging and caching
* in a **single-tier** application, the UI, backend logic and database all reside in the same machine (the user's)
	- examples include desktop applications like Office or image editing software
	- advantages:
		* no latency
		* no data transmission
	- disadvantages:
		* the business has no control over the code -- it's on the user's machine
		* vulnerable to security threats
		* performance, look and feel vary across different machines
* a **two-tier** application consists of a **client** and a **server**
	- the **client** contains the UI and business logic in one machine
	- the **server** includes the database and runs on a different machine, hosted by the business
	- two-tier apps are handy for to-do list apps or other productivity apps, or games, where the security risks are not significant
	- advantages:
		* keeps latency low -- calls only need to be made to the db when the user is ready to persist their changes
	- disadvantages:
		* business logic is susceptible to backwards engineering and subsequent hacking so the use case has to match
* **three tier** applications are popular and widely used. here, each component (UI, business logic and database) are all on different machines and are completely physically separated
* **n-tier** applications have more than three components. they are also known as **distributed applications**
	- components might include
		* cache
		* message queues and other async behaviors
		* load balancers
		* search servers for large amounts of data
		* components involved in processing large amounts of data
		* components running heterogenous tech, commonly known as web services
	- examples include facebook, instagram, uber and other large-scale industry services
	- why?
		* single responsibility principle and separation of concerns/loose coupling
		* because of SRP and SOC ^ this means we can modify/deploy changes to one component without impacting other components (hopefully...)
		* smaller/more manageable repos and apps
* **layers** vs tiers:
	- layers (such as DAL, UI layer, Service Layer, etc) are distinctions between how the code is organized
	- tiers represent physically separated components
	- layers can be used within any one tier of any application

## WEB ARCHITECTURE
* **web architecture** involves multiple components all working in conjunction to form an **online service**

### CLIENT-SERVER ARCHITECTURE
* this architecture works on a request-response model. the client running on a user's device sends a request to the server, and the server responds
* every website is built on the client-server model
* some business websites use peer-to-peer architecture, which is different from client-server

### CLIENT
* the **client** is a mobile app, desktop or tablet, or a web-based console which contains the UI:
	- is typically written in html, js and css
	- is responsible for the look and feel of the application
* types of clients include **thick** and **thin**
	- a thin client holds just the UI with no business logic of any sort. for any action, the client sends a request to the server, just like a three-tier app
	- a thick client contains some or all of the business logic. this is a **two-tier** application

### SERVER
* **server** refers to the machine on which code is executed -- servers which run web applications are known as **application servers**
* other types of server include:
	- proxy server
	- mail server
	- file server
	- virtual server
* server configuration and type depends on the use case and purpose of the server
	- if we are running a java backend application, we would pick apache tomcat or jetty
	- if we are hosting a website, we would pick apache http server
* all components of an application need a server to run -- in modern development, even the UI is hosted on a separate server, which leads us to **server-side rendering**

### REQUEST-RESPONSE
* HTTP is the method of data exchange over the internet
* HTTP is a **stateless** request-response protocol -- meaning each request is executed independently without knowledge or context of previous processes
* requests are commonly made to REST apis

## REST API
* **rest** stands for Representational State Transfer -- an architectural style for web services. Restful architecture implies:
	- client-server design with separation of concerns
	- uniform interface with identifiable resources
	- statelessness: requests are independently executed
	- abstraction of information in the form of resources
* a REST api acts as an interface, exposing **endpoints** to the consuming clients so that they can access resources
* the api does not need to have any concern for the client -- it just needs to expose its resources
* prior t REST apis, different versions of services were needed for each type of client (ex: different version for web vs mobile)
* the REST api acts as a **gateway** to the system -- the api contains business logic, may tie to other apis, authorizes and authenticates, and sanitizes inputs

### AJAX
* **asynchronous javascript and xml**
* AJAX is used for adding async behavior to a web page
* ajax uses an XMLHttpRequest object to send request to the server
* when data is retrieved, the ajax callback can update the DOM without loading the page
* ajax is commonly used with jquery


### PULL AND PUSH
* the default mode of HTTP communication is **HTTP PULL**: the client sends a request and the server responds with data
	- this can be costly: if a client requests information repeatedly but there is nothing new, this is a drain on performance
* **HTTP PUSH** is a response to this wastefulness: the client sends a request for information just once. after the initial request, the server keeps pushing new updates to the client whenever they are available
	- the client doesn't need to send additional requests to get updates
	- the server updates are **callbacks**
	- user notifications are an excellent use case for PUSH
* pushing involves multiple technologies including AJAX long polling, websockets, html5 event source, message queues, etc


### PULL - POLLING WITH AJAX
* data can be pulled/fetched from the server manually as triggered by some event or process; or it can be fetched dynamically at regular intervals via ajax
* using ajax to request information at regular intervals is called **polling**
* for HTTP PULL there is a "time to live" (TTL) which is the limit of time that the browser will wait before closing the connection -- there is an *impersistent connection* which opens and closes for each request attempt


### PUSH
* for push communications we need a **persistent connection**
* the client makes an initial request to the server to establish a connection and **heartbeat interceptors** -- blank request responses that prevent the browser from killing the connection -- keep the connection open
* this is resource intensive but sometimes necessary

#### PUSHING WITH WEBSOCKETS
* a **Web Socket** connection is preferred when we need a persistent bi-directional, low-latency connection
* use cases include messaging, chat apps, social streams, and multiplayer games
* Web Sockets runs over TCP, not HTTP
* the server and client need to support Web Sockets

#### AJAX LONG POLLING
* in long polling, the server holds the response until it finds an update to be sent to the client
* a long polling connection stays open longer compared to polling, despite the server not returning an empty response
* long polling can be used in simple async fetch use cases when you
* smaller number of requests sent, less resource intensive

#### SERVER-SENT EVENTS
* SSE implementation: the server automatically pushes the data to the client whenever updates are available
* data flow is uni-directional
* incoming messages are treated as events
* server can transmit data after the client initiates a connection
* SSE reduces the amount of valueless data transmitted
* on the front end, the HTML5 Event-Source API is used to receive data coming from the server
* SSE is ideal for twitter feeds, stock quotes, real-time notifications etc

#### STREAMING OVER HTTP
* ideal for streaming large chunks of data over HTTP by breaking it into smaller chunks
* uses HTML5 and Javascript Stream API
* client initiates connection and server sends multiple responses back with data
* used for streaming multimedia content, large large images or video

### CLIENT-SIDE VS SERVER-SIDE RENDERING
* the client requests data and the browser receives the response, then rendering it as html on the page
* the browser has several components that all work together to convert the response into an html page
	- browser engine
	- rendering engine
	- js interpreter
	- networking and the UI backend
	- data storage
* **server-side rendering**: to keep the browser from doing all the work, the server can render the UI on the server, generate HTML and then send the html page to the ui
	- server-side rendering is perfect for delivering static content
	- server-side rendering is great for SEO because crawlers can easily read generated content
	- it is not useful in situations where ajax is used to dynamically update the page -- sections or modules have to be fetched and rendered on the fly -- each request generates the entire page on the server
	- this consumes bandwidth/server resources
* standard **client-side rendering** is best for modern dynamic ajax websites
* a **hybrid** approach can be used with server-side rendering for static pages and client-side for dynamic pages


## SCALABILITY
* **scalability** is the ability of the app to handle increased workload without increased latency
* if your app scales well, one request should take as long as one million concurrent requests

### LATENCY
* **latency** is the amount of time it takes for a system to respond to a request
	- in terms of **Big-O Notation** an ideal, scalable app whould have O(1) -- a constant execution time for any input
	- in contrast, an app with a complexity of O(n^2) where n is the size of the data set is not scalable -- the time the algorithm takes to complete will be exponential compared to the input
* latency -- from time of the click to time the system responds -- has two parts:
	- **network latency**: the time the network takes to send a data packet from point A to point B. Content Delivery Networks spread all over the globe or as close to their intended users are possible help reduce network latency
	- **application latency**: the time the application spends processing the request
* latency is important: time is money and latency is a good way to lose a customer

### TYPES OF SCALABILITY
* applications can be scaled in two ways: vertically and horizontally
* **vertical scaling** means adding more power to the server
	- i.e. increased ram on the server
	- also called "scaling up"
	- this is the easy way -- requires no code changes nor complex calculations
	- requires less admin, monitoring and management efforts
	- however there is a limit to how much a server can be scaled up
	- this also introduces increased availability risk compared to horizontal scaling -- the impact of a server going down is much greater
* **horizontal scaling** or "scaling out" means adding more hardware to the existing resource pool -- distributed system
	- there is no limit to how far you can scale out
	- allows for real-time scaling to deal with traffic, taking servers out of rotation
	- however distributed environments require greater management resources
	- horizontal scaling also has specific code demands:
		* no static instances -- they hold application data. rather, persistent memory like a key-value store should be used to hold data and remove all static variables
* **cloud computing** became popular because of its characteristic ability to scale up and down dynamically -- "cloud elasticity"

### BOTTLENECKS
* database: if the db doesn't scale well, neither will the dependent apps. use of partitioning, sharding and multiple db servers will make the db layer more efficient
* application architecture: general mistakes made when determining how the components will interact, such as making everything synchronous
* not using caching: caching can be used at several layers
* inefficient load-balancing
* business logic in the database: this is not the place for it
* using the wrong db: need transactions and strong consistency? use a relational database. need horizontal scalability? use NoSQL (any non-tablular, non-relational db)
* badly written code:
	- unnecessary iteration, executing unnecessary code
	- tightly coupled code
	- not paying attention to big-O complexity ("it handles one request -- how will my app handle 1m requests?")

### IMPROVING SCALABILITY
* tuning the performance of the app
	- **profiling** to see which processes are running long or using too many resources
		* profiling is dynamic analysis and
		* can be used to measure performance
		* helps identify concurrency errors, memory errors and overall robustness/safety
	- **caching**: cache all static content, only hit the db when you need to. use a **write-through cache** which
	- **CDN**: content delivery networks further reduce latency by increasing proximity of the data from the user
	- **data compression**: compressed data uses less bandwidth and takes less time to download
	- **unnecessary requests**: try to club multiple requests into one
* testing scalability:
	- testing can be performed at the hardware and software level
	- simulated traffic is routed to the system to study how the behaves under heavy load
	- anticipate traffic surges

## HIGH AVAILABILITY
* **high availability** is the ability of the system to stay online despite infrastructural failures
* HA is expressed as a percentage -- if a system is 99.9999% available, it is up and available 99.9999% of the time
* you may see the HA percentage in the service level agreement (SLA) of cloud platforms

### REASONS FOR FAILURES
* software crashes: apps crash, corrupt data can bring down an app, memory-hogging processes
* hardware failure: the machine the code runs on fails; overloaded CPU or memory, hd failure, nodes failing; network outages
* human error: the biggest reason for failures: flawed configs, misc PEBCAK
* planned downtime: purposefully taking the system offline for routine maintenance etc

### FAULT TOLERANCE
* **fault tolerance** is a system's ability to stay up to date despite taking hits
* **fail soft**: if some backend nodes go down and some microservices are unavailable, the rest of the app is still functional: for a social media app, the services that upload photos might go down but the page still loads and other content is available
* **microservices** assist in making a system fault tolerance








