# SCALABILITY AND SYSTEM DESIGN FOR DEVELOPERS

## RESOURCES
* [course at educative.io](https://www.educative.io/path/scalability-system-design)
* [Overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
* [RESTful architecture](https://restfulapi.net/)

## SYSTEM DESIGN INTERVIEW: DESIGN SPOTIFY
* use the **reshaded** method
* ask clarifying questions:
	- "how big is the music repo? how frequently is it updated?"
	- "how many (concurrent) users? are all concurrent users streaming or are they doing other less demanding activities?"
	- "are the users globally distributed?"
### REQUIREMENTS
* functional requirements
	- user should be able to **stream** music
	- the system should **store** an archive of music sorted by artist and album
	- the database of the songs should be **searchable**
* non-functional requirements
	- streaming should be very low latency: music should begin playing within 200ms of a user pressing play
	- the system should support a repo of 100M songs

### ESTIMATION
* we will want to store user data as well as versions of songs at different qualities
* assume the average song is 5min and takes up 5mb of space and we have 100M songs
	- medium quality song data at 5mb per song will take 500TB of space: 5mb * 100M = 500,000,000mb = 500,000GB => 500TB
	- low quality song data at 1mb per song will take 500TB of space: 1mb * 100M = 100,000,000mb = 100,000GB => 100TB
	- high quality song data at 10mb per song will take 1000TB of space: 10mb * 100M = 1,000,000,000mb = 1,000,000GB => 1000TB
* hack for estimation: when converting from mb to gb or gb to tb, drop trailing three zeros:
	-> 1000kb => 1mb
	-> 1000mb => 1gb
	-> 1000gb => 1tb
* another example estimating storage for netflix with 10k videos:
	-> 10,000 videos
	-> 1hr average per video
	-> 30gb/hour
	=> 300,000gb => 300TB

### STORAGE
* RDBMS (relational db) vs blob/NoSQL
* what are your data behavior requirements? does the data storage need to be ACIDic and strongly consistent (RDBMS), or do you need vast quantities of data to be available with very low latency (blob)?
* identify what parts of the system need to have strong consistency vs low latency and design storage accordingly
* user data could be stored in a relational db sharded by userid

### HIGH-LEVEL DESIGN
* our workflow will involve:
	- user makes a search
	- search indexer parses data
	- system returns a page of search results
	- user clicks on an item
	- music starts streaming
* identify components that will help you meet requirements: identify services and scaling/caching
	- the song data should be stored and retrieved in chunks and the download will buffer
	- when a user clicks 'play' the first chunk of the song will download and start playing immediately while the rest of the song downloads which should only take about 10 seconds with a 3G (3gb/sec) download speed
	- we could also preemptively download the next songs in the queue/playlist while this song is playing
* we could use write through caching to store misses in the cache
	- caches are ideal for data that is accessed heavily but updated less frequently -- if the data is updated a lot and you're updating the cache every time, you just made a bunch of code for no reason
* a load balancer can distribute traffic across horizontally scaled services


### API
* endpoints will support requirements:
	- GET search/?artist={artistName}&track={title}&genre={genreId}
	- GET titles/{id}
	- GET users/{id}
	- POST users/{id}/playlists/
	- GET users/{id}/playlists/
	- GET users/{id}/playlists/{id}


### DETAILED DESIGN

### EVALUATION

### DISTINCTIVE COMPONENT/FEATURE



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
* essentially a tiered application is a macrocosm of OOO single responsibility principle:
	- each component has "one job"
	- each component has a contract with other components and can be changed internally without impacting others
	- additionally, components that demand higher resources/receive higher traffic can be distributed and scaled without duplicating other components


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
* prior to REST apis, different versions of services were needed for each type of client (ex: different version for web vs mobile)
* the REST api acts as a **gateway** to the system -- the api contains business logic, may tie to other apis, authorizes and authenticates, and sanitizes inputs

### AJAX
* **asynchronous javascript and xml**
* AJAX is a concept, but you'll see it implemented as 'ajax' in libraries like jQuery. axios and other libraries facilitate ajax-like behavior
* AJAX is used for adding async behavior to a web page
* ajax uses an **XMLHttpRequest** object to send request to the server
* when data is retrieved, the ajax callback can update the DOM without reloading the whole page


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
* first, how do web pages do? the client requests data and the browser receives the response, then renders it as html on the page
* the browser has several components that all work together to convert the response into an html page
	- browser engine
	- rendering engine
	- js interpreter
	- networking and the UI backend
	- data storage
* **server-side rendering**: to keep the browser from doing all the work, the server can render the UI on the server, generate HTML and then send the html page to the ui
	- perfect for delivering static/read-only content such as blogs/reports
	- frameworks include ASP.NET Core Razor, ASP.NET Core MVC
	- minimal client requirements: perfect for consumption by low-end devices and low-bandwidth connections
		* support broad range of browser versions
		* quick initial page loads
		* minimal to no js
	- great for SEO because crawlers can easily read generated content
	- protected access of server resources -- keeps db connections and secrets private
	- drawbacks:
		* it is not useful in situations where ajax is used to dynamically update the page: sections or modules have to be fetched and rendered on the fly -- each request generates the entire page on the server
		* this consumes bandwidth/server resources -- higher demand on server overall
* standard **client-side rendering** is best for modern dynamic ajax websites
	- rich interactivity: once loaded, client pulls small, frequent updates from the server. examples: interactive dashboard, drag-and-drop app, social media app
	- frameworks include Angular, React, Vue etc
		* Blazor WebAssembly is a crazy thing that download assemblies and .NET runtime to the browser; the runtime uses js interop to manage DOM and browser API calls
	- supports incremental updates to a page/progressive form completion
	- can be run disconnected with client-side updates eventually synchronizing when connection is reestablished
	- reduced server load -- takes advantage of client devices
	- drawbacks:
		* longer initial load
		* clients with low bandwidth, low-end device, old browser versions may have poor experience
* a **hybrid** approach can be used with server-side rendering for static pages and client-side for dynamic pages


## SCALABILITY
* **scalability** is the ability of the app to handle increased workload without increased latency
* if your app scales well, one request should take as long as one million concurrent requests

### LATENCY
* **latency** is the amount of time it takes for a system to respond to a request
	- in terms of **Big-O Notation** an ideal, scalable app would have O(1) -- a constant execution time for any input
	- in contrast, an app with a complexity of O(n^2) where n is the size of the data set is not scalable -- the time the algorithm takes to complete will be exponential compared to the input
* latency is important: time is money and latency is a good way to lose a customer
* latency -- from time of the click to time the system responds -- has two parts:
	- **network latency**: the time the network takes to send a data packet from point A to point B. Content Delivery Networks spread all over the globe or as close to their intended users are possible help reduce network latency
	- **application latency**: the time the application spends processing the request
* **content delivery network (CDN)**
	- round trip for a response from a node in Virginia to California might take 63ms, but a request to the same node from Cape Town takes 225ms. distributing your services and routing users to the geographically closest service reduces latency
	- a CDN adds complexity but ensures good user experience
	- to set up a CDN, we need to have a routing service that directs data to the correct proxy services based on the location of the request
	- web servers and load balancers need to go through the CDN's routing service before a response can be delivered


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
		* profiling is dynamic analysis and can be used to measure performance
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
* **microservices** assist in making a system fault tolerance:
	- easier management and maintenance
	- easier development: focus on one (relevant) area of code
	- add new features without affecting other services
	- makes system more scalable and highly available
* **redundancy** is a HA mechanism -- we duplicate server instances and keep them on standby to take over if an active server instance goes down -- fail safe backup mechanism in deployment infrastructure
	- this example is known as *Active-passive HA mode* where some nodes are active and some redundant nodes are passive, ready to be switched on if an active node goes down
	- there is also *active-active mode* where all nodes are active by default and some may fail with their traffic being distributed to the remaining nodes
* in the process of achieving HA/fault tolerance, we remove *single points of failure* (aka bottlenecks) by deconstructing the monolith and distributing our systems
	- however it becomes more difficult to achieve a single *synchronous application state* -- but sometimes that's not necessary
* **monitoring and automation** help us identify bottlenecks/failures and helps the system respond automatically without human intervention to maintain uptime
* **kubernetes** is an example of a system that can monitor and intelligently manage instances
* workloads should be **geographically distributed** not only so that globally distrubuted users experience the same low latency, but also so that regional power outages and large-scale failures do not bring the whole system down
* **high-availability clustering**: a HA cluster contains a set of nodes that run in conjunction with one another
	- nodes are connected by a *heartbeat network* that monitors the status of each node
	- a *single state* across all nodes is achieved with *shared distributed memory* and a *distributed coordination service* like Zookeeper
	- HA clusters use disk mirroring/RAID, redundant network connections, redundant electrical power etc -- every component with a single point of failure are made redundant
	- multiple HA clusters run together in one geographical zone

## LOAD BALANCING
* **load balancing** enables our service to scale well and stay highly available when traffic/load increases
* **load balancers** contain the logic and perform the action of load balancing
* load balancers act as intermediaries between the client and the service instances, but there can be multiple layers of load balancing: they can also sit between server instances and db servers
* if a server goes down while processing a request, the load balancer diverts future requests to other active nodes in the cluster
* to know which nodes in the cluster are available for traffic, load balancers must perform regular *health checks* and maintains a running list of which nodes are up and which are down
	- nodes are known as *in-service* or *out-of-service*

### DNS
* to understand how traffic is routed between nodes, we need to understand the **domain name system**
* every machine on the web has a unique **IP address** that allows connections between machines -- IP stands for *internet protocol* and enables delivery of data packets from one machine to another using their IP addresses
* domain name system maps IP addresses to easy to remember domain names
* **DNS querying** occurs when a user types in the URL of a website

[start here](https://www.educative.io/module/lesson/web-application-architecture-101/qV8nq1x9vv7)



## MONOLITH VS MICROSERVICE

### MONOLITHS
* in a **monolithic architecture**, the entire application is contained in a single code base
* monolithic architectures are *tightly coupled*
* pros:
	- easy to build, test and deploy and require less overhead
* cons of monolithic architecture include
	- necessity of deploying the entire application for a change to one part of the system
	- full regression testing due to the tightly coupled nature
	- single point of failure: a bug anywhere might bring the whole thing down
	- maintenance, scalability and HA are all compromised
	- can't leverage heterogenous technologies. want to use Java and NodeJS together in the same codebase? good luck
	- not cloud ready
* monoliths can be a good choice when the requirements and the app are simple and volume/traffic demands are known and low (e.g. internal tool)

### MICROSERVICES
* **microservices** are like a macro version of *SRP (single responsibility principle)*: each service has one clearly focused and cleanly delineated task
* each microservice ideally has its own db, reducing bottlenecks/single points of failure
* pros:
	- no single point of failure
	- heterogenous tech
	- independent and continuous deployment: we can selectively deploy only what's changed without impact to other parts of the system
	- allows for rapid development: we can get the MVP out there and then add more services for additional features, and different developers/teams can work on different parts
* cons:
	- complexity: management, monitoring and deployment are all more complex
	- *strong consistency* is hard to guarantee in a distributed environment -- instead we have *eventual consistency*
* microservices are a good choice for complex use cases where different functionality has different use cases and therefore demands (ex: a social messaging app will require messaging, real-time chat, live video streaming, photo uploads, post likes and shares, etc -- all these have different demands that can be best met by different technology)

[start here](https://www.educative.io/module/lesson/web-application-architecture-101/xlrAgL4D3r3)



