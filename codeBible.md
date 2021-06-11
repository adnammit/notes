# THE BIBLE OF CODING PRACTICES


## TO DO
* try WSL again (esp when WSL2 drops)
* update git
* overhaul config (esp after migrating to WSL)
* keep on doing MovieTime - Vue3


## FUN THINGS TO DO AND KNOW:
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
* heroku for fast back-end prototyping


## QUICK GUIDE OF TERMS AND CONCEPTS
* **abstract class**: a base class containing one or more pure virtual (abstract) functions which must be defined by derived classes. Pure abstract classes contain only abstract member functions and no data. Derived classes can inherit from only one other class. Compare to interfaces
* **antipattern**: a common response to a recurring problem that is usually ineffective and risks being counterproductive
* **arity**: the number of args that a function takes
* **authentication**: is this person who they say they are?
* **authorization**: does this person have permission to access this?
* **Big O**: how quickly runtime grows relative to the input as the input gets arbitrarily large; worst case effort: "this program takes at most x amount of time"
* **blocking/non-blocking**: somewhat synonymous w/ synchronous/asychronous processes but not quite. One of the challenges of isomorphic design: server is blocking, client isn't
* **checksum**: an algorithm (a cryptographic hash function) is run on a piece of data, usually a file. the resulting string can be used to compare to that of other versions of that file to ensure they are the same. checksums are useful for comparing large objects to make sure the application you downloaded is complete and not malicious, to see if a file has been modified (and that an update therefore needs to be run)
* **DDoS**: Distributed Denial of Service: attack via overwhelming a service w traffic
* **declarative programming**: focuses on what the program should accomplish without prescribing how to do it in terms of sequences of operations. Functional programming is declarative. "my address is 1234 S Main St. now you can figure out how to get there". see imperative programming.
* **dependency injection**: one object supplies the dependencies (services) of another object, as opposed to the dependent object building or finding those services itself. For example, client code doesn't need to worry about gathering its dependencies itself, it is only concerned with what to do with that information. When the means of sourcing the dependency changes, the client code does not need to be altered. See low coupling.
* **high cohesion**: the strength of the relationship between the elements of a module. Within a class, the methods and properties share a purpose and function. Correlates with low coupling.
* **imperative programming**: focused on describing how a program operates in terms of a succession of operations that change the program state. "to get to my house, first you take I-35, take exit 43..." etc. see declarative programming
* **interface**: a way of specifying a "contract" that other (people's) code must meet. contains definitions (signatures) of related functionalities (methods, properties, events) that a class or struct can and must implement. A class or struct may implement multiple interfaces. Compare w/ abstract classes.
* **interpolation**: str interpolation: evaluation/expansion of a str literal containing one or more placeholders
* **IoT (Internet of Things)**: system of Things which all have unique identifiers and the ability to transfer info over the internet w/out human-to-human or human-to-computer interaction. Things include computing devices, mechanical systems (like a car with a tire pressure reader), people (someone with a heart monitor implant), and animals (a cow with a biochip transponder) -- anything that can be assigned an IP address and transfer data over a network
* **isomorphic application**: app whose code can run on server or client (ex: JS)
* **low (loose) coupling**: organizing modules of code such that each has a clear function and their interdependence is low, so a fault in one does not break them all. Supports readability and maintainability. Correlates with high cohesion.
* **magic values**: magic values and numbers are anti-patterns and refer to the use of values directly in source code. for example, rather than iterating through every int between 0 and 52, assign variable `deckSize=52`. rather than referring to "John" in code, assign `userName="John"`
* **MITM**: man in the middle: interception of communication between two parties

* **node migrations**:

* **object literal**: in JS, a list of property-value pairs in a comma separated list
* **open-closed principle**: classes should be open for extension and closed for modification
* **ORM**: Object Relational Mapping: conversion of data between incompatible type systems. For example, your DB can only store and manipulate strings and integers, and your client program uses complex objects, so an ORM standardizes mapping data between the two. ORM can be found commercially, and some devs write their own
* **polymorphism**: providing a single interface to different types. in OOP, you may have an abstract class `Vehicle` from which `Car`, `Bicycle` and `Truck` are descended.
* **promise**: an object which represents the completion (success or failure) of an asychronous operation

* **restful interfaces**: Representational State Transfer: uses HTTP protocols (GET, POST etc)


* **singleton**: an object whose instantiation is restricted to one instance (only one is needed or permitted at a time)
* **SQLI**: SQL injection
* **XSS**: cross-site scripting, injecting client-side script in a webpage


## INTERVIEW TIPS
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
    - low coupling: keep components of coding separate so a fault in one does not break them all
    - high cohesion
    - design patterns: book by Martin Fowler
    - algorithms
    - use node in MVC schema
    - have an example of an AJAX request
    - 'database' for a small project
    - react vs jsx vs javascript
    - what is an object literal?
    - what is routing?
    - node migrations
    - dependency injection
    - types vs unit testing
    - databases:
        * SQL query refresh
        * ORM libraries/APIs: Object Relational Mapping - manipulating the data that you get from a DB
* where do you see yourself in five years?
    - mentoring
    - mastery


## GENERAL TOPICS:
* **architecture**: according to [Martin Fowler](https://www.youtube.com/watch?v=DngAZyWMGR0), architecture is the "important stuff": the shared understanding between developers, and the things that are hard to change:
    - architecture is the common understanding of the system design that is shared between expert developers of the system.
        * software development is a social thing -- we have to understand what each other are doing
        * diagrams, design, technical documents are all well and good,but they are just representations of the ideas shared between people writing the software
    - architecture's primary concerns are decisions that are hard to change
    - why good architecture?
        * good design takes more time/money, so how can you argue for it?
        * it's not just about avoiding expense/problems down the line: a well-designed system will allow features to be created more and more rapidly -- saving time/money in the future
* **authentication**: verify the identity of the user
* **authorization**: restrict access to allow only the correct people
* **abstract data type**: a logical, conceptual description of an interface (what it does)
    - list, queue, stack, map, table, hash table
* **data structure**: how the data is stored, the implementation of the ADT (how it does it)
    - array (dynamic, static) [lists, stacks, queues]
    - linear, circular linked lists [lists, stacks, queues]
    - doubly linked lists [lists]
    - BST, red-black tree [table]
    - array of linked lists [hash table]
* **inversion of control**: allowing other code to call you rather
* **dependency injection**: providing an object with its dependencies rather than having it source them itself. Dependency injection is one technique in the broader concept of Inversion of Control
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



## GENERAL C++ RULES FOR KARLA AND BEYOND:
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
* following cin << int, use 'cin.clear()' to avoid endless looping if a
  char is entered.
* operator overloading: don't use void functions! it makes your operator pointless
* operator overloading: copy constructor vs assignment operator overloading
    - karla suggests making both call a copy function
* whenever you create a class in C++, create a copy instructor and overload the assignment operator
* use prefix increment
* karla suggests using 'static' for utility functions from here on out
* check out templates
* Object-Oriented Design: generalities get pushed up into base classes -- the nuances trickle down into derived classes

## EFFICIENCY AND ALGORITHMS:
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


## ELI5: RECURSION:
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



