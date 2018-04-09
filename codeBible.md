# THE BIBLE OF CODING PRACTICES

## FUN THINGS TO DO AND KNOW:
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
* learning about C#? try making a game with Unity
* kaggle: data sets to play with
* high charts: make cool charts in your browser
* use draw.io for diagramming
* heroku for fast back-end prototyping


## INTERVIEW TIPS
* whiteboard: talk through thought process as you assess:
    - syntax, idioms (nuances of the language)
    - algorithms and data structures
    - analytical skills
    - sound design
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
    - react vs javascript
    - what is an object literal?
    - what is routing?
    - node migrations
    - what is dependency injection?
    - databases:
        * SQL query refresh
        * ORM libraries/APIs: Object Relational Mapping - manipulating the data that you get from a DB
    - types vs unit testing
* where do you see yourself in five years?
    - mentoring
    - mastery


## GENERAL TOPICS:
* authentication: verify the identity of the user
* authorization: restrict access to allow only the correct people
* abstract data type: a logical, conceptual description of an interface (what it does)
    - list, queue, stack, map, table, hash table
* data structure: how the data is stored, the implementation of the ADT (how it does it)
    - array (dynamic, static) [lists, stacks, queues]
    - linear, circular linked lists [lists, stacks, queues]
    - doubly linked lists [lists]
    - BST, red-black tree [table]
    - array of linked lists [hash table]
* dependency injection: providing an object with its dependencies rather than having it source them itself
    - dependencies are passed in to a constructor/class definition rather than being instantiated within
    - helpful in that it allows dependencies to be run at runtime - more dynamic
    - useful for testing b/c it allows for dependencies to be mocked up
    - is it good or bad?



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
    - is passing by ref w/ const the best? (saving memory w/out alterations)?
* never pass a class object by value -- very spendy
* send functions the exact type that they expect (aside from trivial conversions) because conversions cause a copy to be made.
* following cin << int, use 'cin.clear()' to avoid endless looping if a
  char is entered.
* operator overloading: don't use void functions! it makes your operator pointless
* operator overloading: copy constructor vs assignment operator overloading
    - karla suggests making both call a copy function
* whenever you create a class in C++, create a copy instructor and
  overload the assignment operator
* use prefix increment
* karla suggests using 'static' for utility functions from here on out
* check out templates
* Object-Oriented Design: generalities get pushed up into base classes -- the nuances trickle down
  into derived classes

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
    - "the little girl couldn't sleep, so her mother told her about a little frog who couldn't sleep, so her mother told her a story about a little mouse who couldn't sleep, so her mother told her a story about a bear who fell asleep, and then the mouse fell asleep, then the frog fell asleep, then the girl fell asleep."
* Droste effect:
    - in art, a pattern that appears recursively within itself
    - a hand drawing a hand drawing a hand...
* tree branches:
    - each branch is self-similar to it's 'parent' branch
