# How To Get a Job

## Things I Know About
* .NET
* REST apis and WCF services
* object oriented programming
* domain-driven design
* layered architecture
* architectural design/systems architecture (tiered systems, microservices)
* design patterns
* testing pyramid
* CI/CD

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
		* interesting project work and research
		* open source experience
		* mobile/web development
		* student groups
		* hackathons
* extracurriculars:
	- student groups

## Elevator Pitch
* as far as my educational experience, after trying on a bunch of different hats and a few different majors, I graduated from Portland State University with a bachelors in Liberal Studies and a minor in Computer Science in 2015
* i got my first job as a programmer making project management software and other Run the Business tools for the medical industry
* i then started working for AltSource, producing custom enterprise solutions, specifically for telecommunications provider ConsumerCellular who I later came to work for directly -- I worked as a developer on the billing team developing their billing, collections and fraud prevention applications. My work was more back-end focused and I contributed on the development of over 60 different applications
* I took a bit of a personal sabbatical starting last summer, then got the wonderful but brief opportunity to work at StackOverflow from January of this year until getting notice of my layoff a few weeks ago -- unfortunately I didn't get to contribute a ton in that time but I did gain more experience in front end performance: optimizing web pages for high request volumes and conducting experiments on modifying the UI to increase user engagement

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

## Interview Tips
* behavioral questions: answer with the STAR method:
	- explain the *situation*
	- describe the *task* you worked on
	- describe the *action* you took: what did *you* do, what was *your* contribution
	- describe the *result*: what did you accomplish and learn
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

## System Design Tips
* check out [this resource](https://www.educative.io/blog/favorite-system-design-question?utm_term=&eid=5082902844932096&utm_campaign=holiday-2022&utm_medium=email&_hsmi=235163542&_hsenc=p2ANqtz-_g02OK8jWnLM8UERg2ahav08M_3z8zLj56Ct-PGSzt1DGdsKQrLE9QtQQ9tc-dPIMJem4NSz0CXvHHixkHztbPT-7DQg&utm_content=&utm_source=educative.io)
* discuss **CAP theorem**
* [educative RESHADED youTube](https://youtu.be/uw-gcK9bjkk)
* [read more about RESHADED](https://www.educative.io/blog/use-reshaded-for-system-design-interviews)

### RESHADED
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


## Discussion Topics
* what makes for maintainable code?
	- follow SRP
	- use descriptive variable names -- comments should be the exception, not the rule
* compare Monolith (no) -> Microservices (better) -> Monorepo of microservices (yes!)
* interface vs abstract class
	- neither interfaces nor abstract classes can be instantiated
	- a derived class may only inherit from one base class, but it may implement multiple interfaces
	- an abstract class may provide base functionality used by the derived classes
	- what's the point of a pure abstract class? it might be easier to extend later on with a virtual property or method
* what is an object literal?
* what is reflection?
* what are examples of antipatterns?
* when would you use a NoSQL database?
* know your acronyms:
	- SOLID
	- CAP theorem
	- ACID
	- IoC
	- DI
	- CRUD vs CQRS
* design patterns: book by Gang of Four
	- singleton: class which only allows one instance of itself to be created; manages concurrent access to a single resource (such as logging, caching, configuration); may be lazy loaded
	- factory: decouple/abstract means of creating objects from the client; a means of creating objects without specifying the exact class that will be created using inheritance; way of centrally managing implementation details and ensuring objects are always created in the correct state

### Algorithms
* Big O notation: "this algorithm takes at most x amount of time". identify algorithms as being:
	- constant (pop, dequeue)
	- logarithmic (divide and conquer)
	- linear (foreach in data set)
	- quadratic (nested for loops)
* recursion
* powers of 2 ?

### Testing
* types vs unit testing
* testing pyramid

### Design
* how do you scale large enterprise applications and make them scale in-flight?
* low coupling and high cohesion
* dependency injection
* Object Oriented Design
* DDD/Layered architecture

### Data
* acidity
* normalization

### Security
* avoiding sql injections and other security topics (typing and parameterized sql, ORM, principle of least privilege)
* prevent "do stuff" and "get stuff" Cross Site Request Forgery (CSRF)


## Questions to Ask

### Position and Company
* Contract vs Direct Hire
* Remote/Hybrid policy: coworker timezones, working agreement (how do you maintain work life balance?)
* How is the org/team structured and how would I fit in?
* What disciplines are included on a team? (qa, product) is there a dedicated architecture team or team that provides big-picture solutions guidance?
* What opportunities for professional development are available?
* what does the onboarding process look like?
* What would a typical day be like in this position?
* What does the company do to help its employees maintain work-life balance?
* How is work tracked? What project management methodology is used? (Agile Scrum/Kanban or waterfall?)
* How are projects prioritized? (how does tech debt fit in?)
* How is documentation maintained? (confluence, wiki, etc?)
* Do you have any questions or are there any ambiguities about my experience that I can explain or add more detail to?

### Engineering Questions
* What is the tech stack? Is it OS-agnostic?
* What are the ideologies or patterns that are most important? (DDD?)
* What tools do you use?
* Any current plans or initiatives to use AI/ML?
* How collaborative is the team? How are peer programming and code review used? are code reviews a more formal part of the integration process or more informal?
* What is the deployment schedule? Do you employ CI/CD or have a regular release schedule based around sprints?
* How is QA structured? what parts of the testing pyramid (unit/integration/e2e) would I be responsible for?
