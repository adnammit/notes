# The (Coding) Bible

[[toc]]

## To Do
* update git
* learn:
	- hangfire, rabbitmq
	- kubernetes
	- snap ins
	- entity
	- azure/t-sql
	- sql review
	- aws
	- more node.js, express
	- scala, haskell
	- docker
	- maths: fractions, ratio, proportions, linear equations

## Fun Things To Do And Know
* check out fastcomet for webhosting
* make an agent-based model to graphically display the growth of a search tree
* probabilistic data structures
* learn what the hell sed is
* learn more about security
* restful web services
* APIs
* how to authenticate people
* get involved with open source
* learn more about the DOM
* check out quora (like stack overflow, but different)
* use code pen
* learning about C# -- try making a game with Unity
* kaggle: data sets to play with
* high charts: make cool charts in your browser
* use draw.io for diagramming
* heroku/render/netlify for fast back-end prototyping

## Things I Already Know
* .NET
* REST apis and WCF services
* domain-driven design/object oriented programming
* architectural design/systems architecture (tiered systems, microservices)
* design patterns
* testing pyramid
* CI/CD Pipeline: Continuous Integration: developers merge code changes in a central repository multiple times a day. Continuous Delivery: automation of software release via infrastructure provisioning and deployment, which may be manual and consist of multiple stages

## Algorithm Interview Tips
* brush up on fundamentals: string and array manipulation
* practice coding -- don't look rusty
* look for patterns:
	- two pointers
	- stacks
	- subsets
	- cyclic sort
	- union find
	- k-way merge
* interviewers are looking for signals that you're a good hire:
	- what design considerations are you making?
	- how do you generate your test cases?
	- what questions do you ask?
* interviewers may give you complexity/efficiency requirements up front (i.e. "the data is not sorted and cannot be sorted" so you know you're looking for a linear time solution) - otherwise you might code a sub-optimal solution and then try to iterate on it or at least point out the pitfalls

## General Interview Tips
* behavioral questions: answer with the STAR method:
	- explain the *situation*
	- describe the *task* you worked on
	- describe the *action* you took: what did *you* do, what was *your* contribution
	- describe the *result*: what did you accomplish and learn
* system design
	- **RESHADED** approach ([read more](https://www.educative.io/blog/use-reshaded-for-system-design-interviews))
		- **requirements**: clarify the functional and non-functional requirements
			* functional ex: users need to stream data; system should store a sortable, searchable archive of music
			* non-functional ex: streaming should be low-latency -- a song should start within 200ms of clicking play; we need to store 100 million songs
		- **estimation**: what are the hardware or infrastructural requirements necessary
			* how many users? how many concurrent users? are they globally distributed? mobile and web?
			* how much data do we need to store? how often is it updated?
		- **storage schema (optional,last)**: articulate the data model -- define tables, fields and demonstrate granular understanding of the system and make sure the system can handle the data efficiently
			* this is somewhat optional and may be done after everything else is complete
			* particularly focus on:
				- when the data is highly normalized
				- when different parts of the data need to be stored in different formats
				- when there are performance or efficiency concerns around how the data is stored
		- **high-level design**: identify the main components and building blocks of your system -- start to focus on fulfilling the functional requirements
			* don't get bogged down in details. keep it high level -- you can iterate and improve as you continue to work
			* outline the relationship between clients (mobile and desktop), load balancers, application servers, and database instances -- add in message queues, caches, etc as needed
		- **APIs**: design the interface that will be used by the client. determine the API calls that will be needed to meet the functional requirements
			* ex:
				```
					get all items: 		GET /items
					get all user items:	GET /users/{userId}/items
					add user item: 		POST /users/{userId}/items
				```
		- **detailed design**: refine and finalize the design of your system
			* call out technology choices
			* analyze the high-level design and discuss details to improve the final design
			* add components that meet the non-functional requirements
			* ex: discuss why you chose a NoSQL db over a relational db
			* discuss:
				- what is the availability of your system? is it fault tolerant?
				- how is caching being handled?
				- how are load balancing and CDNs being used to reduce latency and distribute load?
		- **evaluation**: does your design effectively meet the functional and non-functional requirements, and have you made justifications for the design you chose?
			* address any trade-offs you made and how you weight the benefits and drawbacks of other solutions
			* identify aspects of your system design that could be improved on
		- **distinctive component/feature**: you may need to add a unique feature to your design to meet the requirements, such as a concurrency control mechanism to support multiple concurrent users
	- check out [this resource](https://www.educative.io/blog/favorite-system-design-question?utm_term=&eid=5082902844932096&utm_campaign=holiday-2022&utm_medium=email&_hsmi=235163542&_hsenc=p2ANqtz-_g02OK8jWnLM8UERg2ahav08M_3z8zLj56Ct-PGSzt1DGdsKQrLE9QtQQ9tc-dPIMJem4NSz0CXvHHixkHztbPT-7DQg&utm_content=&utm_source=educative.io)
	- focus on **CAP theorem**

* whiteboard: talk through thought process as you assess:
	- syntax, idioms (nuances of the language), proficiency
	- algorithms and data structures
	- analytical skills: logic and process to get to the solution
	- sound design
		* define interfaces
		* identify bugs and efficiency concerns
		* discuss tradeoffs for potential solutions
	- correctness: does it compile and run?
	- quality: clean, testable, well-named vars and funcs
* questions/topics:
	- reading/refactoring code
	- what makes for maintainable code?
	- types vs unit testing
	- singleton: class which only allows one instance of itself to be created
	- interface vs abstract class
	- Big O notation: "this algorithm takes at most x amount of time"
	- recursion
	- powers of 2 ?
	- low coupling and high cohesion
	- design patterns: book by Martin Fowler
	- algorithms
	- use node in MVC schema
	- have an example of an AJAX request
	- react vs jsx vs javascript
	- what is an object literal?
	- what is routing?
	- node migrations
	- dependency injection
	- avoiding sql injections and other security topics
	- types vs unit testing
	- OOO
		* SOLID
	- databases:
		* SQL query refresh
		* ORM libraries/APIs: Object Relational Mapping - manipulating the data that you get from a DB
		* acidity
	- how do you scale large enterprise applications and make them scale in-flight?
* where do you see yourself in five years?

## Vocab and Concepts
* **abstract class**: a base class containing one or more pure virtual (abstract) functions which must be defined by derived classes. Pure abstract classes contain only abstract member functions and no data. Derived classes can inherit from only one other class. Compare to interfaces
* **ACID**: Atomicity, Consistency, Isolation and Durability; key feature of relational databases
* **antipattern**: a common response to a recurring problem that is usually ineffective and risks being counterproductive
* **arity**: the number of args that a function takes
* **authentication**: verify the identity of the user. is this person who they say they are?
* **authorization**: restrict access to allow only the correct people. does this person have permission to access this?
* **availability**: high availability means the system remains in operation at all times
* **Big O**: a type of Asymptotic notation that describes how quickly runtime grows relative to the input as the input gets arbitrarily large; worst case effort: "this program takes at most x amount of time"
* **blocking/non-blocking**: somewhat synonymous w/ synchronous/asychronous processes but not quite. One of the challenges of isomorphic design: server is blocking, client isn't
* **boxing and unboxing**: converting a value type to a reference type and vice versa (respectively). considered bad practice
* **CAP theorem**: a distributed system cannot simultaneously be consistent, available, and partition tolerant -- you must choose two of the three given your use case
* **checksum**: an algorithm (a cryptographic hash function) is run on a piece of data, usually a file. the resulting string can be used to compare to that of other versions of that file to ensure they are the same. checksums are useful for comparing large objects to make sure the application you downloaded is complete and not malicious, to see if a file has been modified (and that an update therefore needs to be run)
* **CLR**: Common Language Runtime: the execution engine that handles running .NET applications compiled to Intermediate Language. it includes a JIT compiler to convert IL to machine/native code
* **compiler**: takes source code written in one language and produces an output file in another language -- the output is generally a binary executable written in machine code, but transpilers (source-to-source) can be considered a subset of compiler
* **consistency**: all nodes in a system see the same data simultaneously
* **correctness**: does the program behave as expected?
* **DDoS**: Distributed Denial of Service: attack via overwhelming a service w traffic
* **declarative programming**: focuses on what the program should accomplish without prescribing how to do it in terms of sequences of operations. Functional programming is declarative. "my address is 1234 S Main St. now you can figure out how to get there". see imperative programming.
* **dependency injection**: one object supplies the dependencies (services) of another object, as opposed to the dependent object building or finding those services itself. For example, client code doesn't need to worry about gathering its dependencies itself, it is only concerned with what to do with that information. When the means of sourcing the dependency changes, the client code does not need to be altered. See low coupling.
* **eventual consistency**: in a distributed system, it may take time for an update to be reflected across all data nodes. this is usually a requisite trade off for high availability/low latency. see strong consistency
* **high cohesion**: the strength of the relationship between the elements of a module. Within a class, the methods and properties share a purpose and function. Correlates with low coupling.
* **horizontal scaling** or "scaling out" means adding more hardware to the existing resource pool in a distributed system. there is no limit to how far you can scale horizontally and it's easier to maintain uptime by rotating servers, though it does require more overhead to manage and different coding considerations
* **idempotent**: if a process is idempotent, it can be re-run multiple times and will still result in the same end state as existed after the first run (multiple applications of the process == the first application of the process) -- ex: this is important in REST apis: if you POST the same request multiple times, you would hope not to have multiple objects
* **imperative programming**: focused on describing how a program operates in terms of a succession of operations that change the program state. "to get to my house, first you take I-35, take exit 43..." etc. see declarative programming
* **interface**: a way of specifying a "contract" that other (people's) code must meet. contains definitions (signatures) of related functionalities (methods, properties, events) that a class or struct can and must implement. A class or struct may implement multiple interfaces. Compare w/ abstract classes.
* **interpolation**: str interpolation: evaluation/expansion of a str literal containing one or more placeholders
* **IoT (Internet of Things)**: system of Things which all have unique identifiers and the ability to transfer info over the internet w/out human-to-human or human-to-computer interaction. Things include computing devices, mechanical systems (like a car with a tire pressure reader), people (someone with a heart monitor implant), and animals (a cow with a biochip transponder) -- anything that can be assigned an IP address and transfer data over a network
* **isomorphic application**: app whose code can run on server or client (ex: JS)
* **JIT Compiler**: just-in-time compiler is a component of the .NET CLR, turning Intermediate Language into machine code
* **KPI**: Key performance indicator: a quantifiable measure of performance
* **lazy-loading**:
* **low (loose) coupling**: organizing modules of code such that each has a clear function and their interdependence is low, so a fault in one does not break them all. Supports readability and maintainability. Correlates with high cohesion.
* **LTS**: long term support: some versions are supported longer than others; if you're frequently updating a project and want the cutting edge, use the latest, but if you need something stable that you can set and forget (for a bit) select a LTS version
* **magic values**: magic values and numbers are anti-patterns and refer to the use of values directly in source code. for example, rather than iterating through every int between 0 and 52, assign variable `deckSize=52`. rather than referring to "John" in code, assign `userName="John"`
* **managed code**: code which is managed by a runtime at execution: the CLR compiles managed code to machine code, then executes the machine code. for **unmanaged code** the dev must manage execution and security concerns, handle garbage collection, target certain hardware architectures, etc
* **MITM**: man in the middle: interception of communication between two parties
* **node migrations**:
* **object literal**: in JS, a list of property-value pairs in a comma separated list
* **open-closed principle**: classes should be open for extension and closed for modification
* **ORM**: Object Relational Mapping: conversion of data between incompatible type systems. For example, your DB can only store and manipulate strings and integers, and your client program uses complex objects, so an ORM standardizes mapping data between the two. ORM can be found commercially, and some devs write their own
* **partition tolerance**: a partition is a break in communication between nodes - a partition tolerant system does not fail regardless of dropped messages between nodes
* **polymorphism**: providing a single interface to different types. in OOP, you may have an abstract class `Vehicle` from which `Car`, `Bicycle` and `Motorcycle` are descended.
* **promise**: an object which represents the completion (success or failure) of an asychronous operation
* **restful interfaces**: Representational State Transfer: uses HTTP protocols (GET, POST etc)
* **restful api**: Representational State Transfer: a set of standards which includes an api interface to allow other apps to interface with it, statelessness, scalability, cacheability and interface uniformity
* **semaphore**: a variable or abstract data type used to control access to a common resource by multiple threads
* **singleton**: an object whose instantiation is restricted to one instance (only one is needed or permitted at a time)
* **SOLID**: an object-oriented programming paradigm; Single Responsibility, Open-closed, Liskov substitution, Interface segregation, Dependency inversion
* **SQLI**: SQL injection
* **strong consistency**: "one source of truth" -- data may be backed up in multiple places but it is read/written in one location to ensure the data is always 100% accurate; no dirty reads. see eventual consistency
* **testing pyramid**: three tiers from most basic/fastest to most complex/slowest
	- **unit tests**: the foundation of testing; limited, focused scope: do the code units function as intended? each unit test should test a single variable
	- **integration tests**: the second layer: how do the units integrate with the rest of the code? requires a pre-prod environment to run. ex: (maybe) postman test to check that mods to a service still work
	- **end-to-end tests**: the top of the pyramid: does the whole application work? what is the user experience? feature and regression testing are usually e2e tests
* **transpilation**: transpilers are source-to-source compilers -- they take code written in one source language and convert it to another source language (e.g. Babel might transpile ES6+ to ES5)
* **vertical scaling** means adding more power to the server: more RAM, more CPU etc. this works in a pinch, but is limited in how far you can scale and the impact of a server going out is great
* **write-through cache**: a storage method in which data is written into the cache and corresponding main memory location at the same time. cache sits between db and application - all operations pass through cache tier so anything retrieved from or updated to the db is stored in the cache. this is a nice compliment to **lazy loading**
* **XSS**: cross-site scripting, injecting client-side script in a webpage


## Topic Deep Dives
* **architecture**: according to [Martin Fowler](https://www.youtube.com/watch?v=DngAZyWMGR0), architecture is the "important stuff": the shared understanding between developers, and the things that are hard to change:
	- architecture is the common understanding of the system design that is shared between expert developers of the system.
		* software development is a social thing -- we have to understand what each other are doing
		* diagrams, design, technical documents are all well and good,but they are just representations of the ideas shared between people writing the software
	- architecture's primary concerns are decisions that are hard to change
	- why good architecture?
		* good design takes more time/money, so how can you argue for it?
		* it's not just about avoiding expense/problems down the line: a well-designed system will allow features to be created more and more rapidly -- saving time/money in the future
* **inheritance**
	- **polymorphism**: providing a single interface to different types. in OOP, you may have an abstract base class `Vehicle` from which `Car`, `Bicycle` and `Motorcycle` are descended
	- **abstract** classes serve as the **base** class blueprint for **derived** classes and may implement virtual methods that must be overridden by the implementing base class
	- you cannot create an instance of an abstract class but you can implement functionality that will be shared by inheriting classes
	- abstract classes cannot be static
* **abstract data type**: a logical, conceptual description of an interface (what it does)
	- list, queue, stack, map, table, hash table, dictionary
* **data structure**: how the data is stored, the implementation of the ADT (how it does it)
	- array (dynamic, static) [lists, stacks, queues]
		* data is stored in contiguous locations making iteration efficient -- we can use indexing
		* pro: lower overhead
		* con: not flexible; if you want to add/remove, you'll have to create a new one
	- linear, circular linked lists [lists, stacks, queues]
		* data is *not* stored in contiguous locations -- no index, instead we use nodes and pointers to other nodes
		* `head` and `tail` refer to first and last node
		* in C#, if you need to "slice" a linked list, you can just set a pointer to null and garbage collection will take care of the rest
		* pro: these are flexible -- dynamically add or remove items
		* con: less efficient than array, more overhead
	- doubly linked lists [lists]
	- BST, red-black tree [table]
	- array of linked lists [hash table]
* **inversion of control**: allowing other code to call you rather than your code managing everything
	- steps for a nice IoC:
		-> Tightly coupled classes
		-> implement IoC using factory pattern
		-> implement DIP through abstraction
		-> implement DI
		-> use IoC container
		-> loosely coupled classes
* **dependency injection**: providing an object with its dependencies rather than having it source them itself. Dependency injection is one technique in the broader concept of Inversion of Control (or *dependency inversion*)
	- dependencies are passed in to a constructor/class definition rather than being instantiated within
	- helpful in that it allows dependencies to be run at runtime - more dynamic
	- benefit that the client (dependent) object doesn't need to be changed when the method of acquiring the dependencies changes
	- useful for testing b/c it allows for dependencies to be mocked up more easily
	- dependency injection consists of four roles:
		* the **service** object(s) to be used
		* the **client** that depends on the services
		* the **interfaces** that define how the client may use the service
		* the **injector** which constructs and injects the services
	- there are three paradigms for accepting injection:
		* setter:
		* interface: the client has the opportunity to control its own injection
		* constructor
	- pros:
		* system may be reconfigured without recompilation of the client
		* separate configurations can be written for different situations including testing
		* the ability to create stubs and mock objects makes for easier unit testing
		* since client has no concept or control over changes in dependencies, it is isolated from changes in design or defects elsewhere, thus promoting reusability, testability and maintainability. Coupling is decreased between the client and its services
		* allows for concurrent development: one dev can work on the client, another on the injector, as long as the interface is already agreed upon
	- cons:
		* can be more difficult to read, trace and debug -- more files must be read
		* demands more upfront development effort (maintaining two things is harder than maintaining one)
* **SOLID** OOO design principals. it is important to use these principals so that one area of software can be modified without impacting other areas. it makes the design easier to understand, maintain and extend. it also makes for easier testing, and more agile, adaptive, bug-free software
	- *Single-Responsibility principle*: each class should have one responsibility, one reason to change. each class should solve one problem. this applies to microservices as well -- we can have one service for auth and another for importing client data so one service isn't doing it all. this makes code simpler and easier to modify, but it does require more code to be written
	- *Open-Closed principle*: entities should be open for extension but closed to modification (via inheritance)
	- *Liskov substitution principle*: every subclass should be substitutable for their base or parent class -- you should be able to substitute an instance of a derived class for an instance of its base class with no errors
		* **variance**
		* **covariance**
		* more [here](https://youtu.be/-3UXq2krhyw)
	- *Interface segregation principle*: clients should not be forced to depend on interfaces or methods that they do not use. interfaces should be fine-grained and client specific so that clients can implement only those needed
		* if your concrete class has to implement methods with `notImplException` or with default values for properties that don't apply just to satisfy the interface, the interface isn't fine-grained enough
		* example: if you type all your library items as `IBook` with fields for pages, that won't apply to your audiobook or cd. fine grained nested interfaces will help define only what is necessary for your use case
	- *Dependency inversion principle*: depend on abstractions, not concretions. “abstractions should not depend on details. Details should depend upon abstractions.” this allows for decoupling. a technique to implement DIP is *dependency injection*
		* high level modules (modules that depend on other things)
		* low level modules (modules that depend on nothing)
		* abstractions (interfaces) help high and low level modules work together and allow for changes to be made to low-level modules without breaking high-level modules. e.g. if your application depends on `IMessageService` you can inject an `EmailService` or an `SmsService` or pass in a mock service for testing that won't send real messages
		* if your high-level code creates a lot of `new` instances, it is dependent on that low level module. you have to create new instances somewhere, and that place should be centralized so you're swapping all your stuff out in one place and can supply your dependencies from there (i.e. factory)
* **.NET ECOSYSTEM**:
	- **.NET generally speaking**:
		* .NET is a developer platform made up of tools, programming languages, and libraries for building many different types of applications
		* The base platform provides components that apply to all different types of apps and multiple languages, including basic libraries (strings etc), editors, and the C#, F# and Visual Basic programming languages
		* all .NET programs are compiled into a CIL (Common Intermediate Language) that is then translated to machine code and executed
			- when the app is built, regardless of what language the app is written in, it all gets compiled into a common "dotnet speak" -- the IL (intermediate language) is stored in the .exe or .dll generated by the build
			- when the app is executed, the CLR's just in time (JIT) compiler converts the intermediate language to machine code -- there is a header in the executable that loads the .NET CLR, and the .NET CLR JIT compiles the IL to native code for the CPU. the native code is cached for CPU execution
			- this is how you're able to run programs written in many different languages without your computer "speaking" F#
			- the CLR includes services such as thread management, garbage collection, type safety and exception handling
	- **.NET Framework**:
		* the original implementation of .NET
		* supports development of websites, services, desktop apps and more
		* only supports Windows
		* includes the CLR and Class Library
		* Framework is currently at 4.8 and will be supported but will not be developed further
	- **.NET Core**:
		* an open-source, cross-platform reimagination of the common layer below all languages -- the basis of Core is the CLR Core
		* this strips a lot of backwards compatibility for things we don't need anymore -- this fresh start allows for much greater performance
		* Core still uses the same abstractions, same languages as .NET Framework instances that compile to a CLR Core, it just does a lot less stuff, faster
		* by Core 3, we have most of the functionality that we have in .NET
		* still missing some things in Core like WCF (which is still supported in Framework)
		* confusingly, we skipped Core 4 and what would be Core 5 is now known as just .NET 5 🙄
		* Core can be used with Docker, and it includes command line tools for local development and continuous integration
	- **ASP.NET**:
		* ASP.NET extends the .NET platform with tools and libraries specifically for building web apps
		* includes
			- request processing framework
			- Razor web page templating
			- common web pattern libraries like MVC
			- authentication system
			- editor extensions for working with web pages
	- **.NET Standard**:
		* Standard is a specification of .NET, rather than an implementation -- it is a successor of the portable class library
		* it defines the set of APIs that all .NET implementations must provide
		* kind of like another .NET Framework, but it is only used to develop class libraries -- it is a "bridge" between these different abstraction layers
		* example: you use Standard to write a shared library that is consumed by both Framework and Core applications (specifically you use Standard 2.0 as Standard 1.x is not supported by Core)
		* Standard can also be used to "bridge" between things like Xamarin, Mono and Unity
		* Standard will eventually be obsolesced once we no longer need to cross the bridge -- since the new .NET 5+ will be cross-platform compatible, mobile etc, we won't need to translate anymore
	- **Xamarin/Mono**: .NET implementations for mobile OS

## Database Stuff
* **indexing**
* **normalization** is the process of (1) identifying redundant data within a table and (2) enhancing data integrity. normalization also helps organize data
	- normalization organizes the columns and tables in a db to endure constraints properly execute their dependencies
	- normalization is based on functional dependency
	- normalization/data redundancy reduction has these advantages:
		* less likely for there to be inconsistency between the dupe data
		* data maintenance is less tedious -- data is maintained in fewer places
		* fewer anomalies caused by failing to synchronize data
		* less disk space
* **CRUD**: Create, Read, Update and Delete
* **join types**
	- **inner** most performant, common rows between both tables
	- **left outer** contains all the data from the first table and the common rows from the second table with null values where there is no data in table 2
	- **right outer** same as left but all data from table 2 and matching data from table 1
	- **full join** combines and returns all data from both tables, regardless of whether there is shared information with nulls where there is no match between tables
	- **cross join** is WILD. for each row in table 1, join it with every row in table 2. if table 1 has x rows and table 2 has y rows, the resulting set will contain x * y rows. comes in handy if you need all possible combinations of two different objects together (hopefully small sets of data)
* **ACID**: ACID transactions are a key feature of relational databases
	- **Atomicity**: each transaction will either succeed completely or fail completely; if any component statement fails in a transaction, the whole transaction fails (e.g.: in a bank transfer, some failure doesn't result in both accounts or neither account having the money). to an observer of the system, the transaction has not occurred one moment and has fully occurred the next moment
	- **Consistency**: data updated via transactions will respect other constraints and rules to keep the data in a consistent state. corruption or errors in your data will not create unintended consequences for the integrity of the table
	- **Isolation**: transactions may run concurrently. isolation of transactions ensures that concurrent transactions do not interfere with or effect one another. if you were to take the transactions and run them one-by-one, the result would be the same as though they ran concurrently
		* this is generally achieved by locks/isolation level which varies by application
		* the order your bank transactions post matters but your twitter feed order does not
		* a **schedule** is an interleaving of transaction actions -- the order of operations within each transaction must be preserved
	- **Durability**: ensure that changes made to data by successful transactions will be saved, even in the event of system failure
	- ACID pros:
		* data integrity
		* simplified operational logic -- complex updates don't require advance examination of the mutual interaction mechanisms
		* reliable storage: in-memory storage fails, durable storage is more reliable
	- ACID cons:
		* ACID transactions are slower because of locking
	- Alternatives to ACID:
		* for high-volume systems (netflix, facebook), distributed systems working in parallel perform better
		* NoSQL and MongoDB replicate data across several nodes or servers where each node contains the same data, but don't update the data at the same time across all nodes, so data can be inconsistent
		* high availability of distributed db systems comes with the tradeoff of lower consistency

### Locks
* a transaction must get a lock before it can read or update an item -- multiple transactions can operate on the same db but must lock individual items
* **shared lock** (s) can be used for reading
* **exclusive lock** (x) for writing. to get an x lock, there can be no locks of any type
* locks are managed by a **lock manager**
* if a transaction cannot get a lock, it waits in a queue
* locking protocols:
	- **strict 2-phase protocol**: states that locks cannot be released until the transaction ends via commit or abort
	- **non-strict 2-phase protocol**: a transaction gets and drops locks as needed
* **locking scopes** can help improve concurrency
	- you can lock tables, rows, attribute values, or predicates (such as age > 25 -- this one is difficult to implement)
	- more granular locks -> increasing concurrency
* **deadlock**: strict 2-phase locking protocol may result in deadlock: queued transactions are waiting for an abort/commit that will never happen, e.g.:
	- T1 wants a X lock on A and has a X lock on B
	- T2 has a X lock on B and wants a X lock on A
	- solution: the DBMS must detect this scenario and kill one of the requests -- one transaction is the 'victim' of deadlock
* using `NOLOCK` for a query seems like a good idea if you don't want to change any data, so why might that be a bad idea?
	- using `NOLOCK` means another query won't block your access to that data, but if you read that data while the query is changing it, you might get a **dirty read** where your data is suddenly no longer current
* consider `NOLOCK` vs `SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED`
	- `NOLOCK` sets just one table to read w/out locking
	- the latter sets the state of the entire transaction

### Performance
* proper indexing
* avoid using `SELECT *` -- specify the exact columns you want. this helps because:
	- slims down your data fetch and reduces network traffic
	- possible to retrieve two columns of the same name when using joins
	- `SELECT * with WHERE` conditions will use clustered index by default so it might not use other more optimal indexes
* use temporary tables rather than iterating over the same beastly table
* if you have the options of using a vw or a sproc, sprocs provide more ways to optimize
* use dynamic joins wherever possible (only join if you have to)
* inner joins are faster than outer joins (smaller data set)
* if you have to use cross join, avoid performance issues:
	- Use another JOIN (INNER/LEFT/RIGHT) with 2 ON conditions
	- Use the GROUP BY clause to pre-aggregate data
* use more `WHERE` conditions to narrow down your pool (such as only recent data, etc) over `HAVING`
* enums take longer than bools
* query plans are cached -- if the query parameters are exactly the same, it pulls the existing query out of the cache and uses that one. "optimized" queries sometimes waste time -- the cache may need to be cleared
* avoid running queries in a loop -- try to insert what you need in a temp table if possible
* don't put `CONST`s at the beginning of a sproc


## General C++ Rules For Karla And Beyond
* no global variables!
* never dereference a null pointer!
* use local variables rather than dereferencing a million times
* do not include namespace std in the .h file (why?)
* use break and return sparingly -- effective use of if/else loops
* pass by ref as much as possible
* marking void as void
* avoid passing char[] -- pass char* instead to save mem?
* const: "this function won't change any of the class's data members"
* passing by reference vs. passing by value vs. passing by const reference
	- is passing by ref w/ const the best when you're not changing the data that's being passed in (saving memory w/out alterations)?
* never pass a class object by value -- very spendy
* send functions the exact type that they expect (aside from trivial conversions) because conversions cause a copy to be made.
* following cin << int, use 'cin.clear()' to avoid endless looping if a char is entered.
* operator overloading: don't use void functions! it makes your operator pointless
* operator overloading: copy constructor vs assignment operator overloading
	- karla suggests making both call a copy function
* whenever you create a class in C++, create a copy instructor and overload the assignment operator
* use prefix increment
* karla suggests using 'static' for utility functions from here on out
* check out templates
* Object-Oriented Design: generalities get pushed up into base classes -- the nuances trickle down into derived classes

## Efficiency And Algorithms
* the running time of an algorithm is a function of the size of its input
* we look at the worst case: for example, if we're searching for a match, the worse case would be iterating through all of the data without finding a match
* kinds of functions:
	- constant: a, 10, 283.4
	- logarithmic: log2, ln
	- linear: an + b
	- quadratic: an^2 + bn + c
	- polynomial: an^z + ... + an^2 + an^1 + an^0
	- exponential: a^n
* this estimated time is expressed as a function of input (n), listed from slowest to fastest growing:
	- constant: O(1)
	- logarithmic: O(ln n)
	- linear: O(n)
	- O(n ln n)
	- exponential/quadratic: O(n^2)
	- O(n^2 ln n)
	- polynomial: O(n^a)
	- exponential: O(2^n)
* Asymptotic notation:
	- when we measure the rate of growth of an algorithm's running time, we drop the less important factors in the calculation and focus on the most significant.
	- suppose that an algorithm takes 2n^2 + 6n + 8 machine operations to complete, where n is the input. the most significant part of the equation is n^2, so that is how the algorithm's efficiency is simplified
* three forms of Asymptotic notation:
	- Big Theta: the rate of growth is bound between constant factors above and below -- we know that our algorithm performs somewhere in between
		* "this algorithm takes at least x amount of time and at most y amount of time"
	- Big O: the upper boundary of growth of our search algorithm
		* "this algorithm takes at most x amount of time"
	- Big Omega: the lower boundary of growth of our search algorithm
		* "this algorithm takes at least x amount of time"

## ELI5: Recursion
* matryoshka doll:
	- self-similar, nested layers that repeat until the problem has been 'solved' (you reach the innermost doll)
* researching something on the internet:
	- you open a tab and it tells you something, but now you have a question about something else so you pause what you're learning about and open a new tab, etc until you don't have any more questions and you work your way back through the tabs until you have a full understanding of what you wanted to know
* the bedtime story:
	- "the little girl couldn't sleep, so her mother told her about a little frog who couldn't sleep, so her mother told her a story about a little mouse who couldn't sleep, so her mother told her a story about a little bear who fell asleep, and then the mouse fell asleep, then the frog fell asleep, then the girl fell asleep."
* Droste effect:
	- in art, a pattern that appears recursively within itself
	- a hand drawing a hand drawing a hand...
* tree branches:
	- each branch is self-similar to it's 'parent' branch


## Resume Tips
* clean and simple
* action words! what was your impact?
	- created, designed, debugged
	- focus on results and impact - list metrics rather than long descriptions
* use bullet points
* include github with your contact info
* one page - eh, not so much
* education
	- should be first if you don't have work exp
	- list gpa, etc, key courses completed; algorithms, etc
	- brief description of important projects
	- technical skills
* experience:
	- chronological order
	- employer, position, dates
	- concise descriptions
	- showcase results for:
		* internships
		* interesting project work and research (robby?)
		* open source experience
		* mobile/web development
		* student groups (?)
		* hacks coding competitions
* extracurriculars:
	- student groups