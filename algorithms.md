# ALGORITHMS

## ASSUMPTIONS
* assumptions are critical to algorithmic accuracy and performance
* if you can pin down assumptions, you can reduce the number of operations that are needed in your algorithm which can really add up over n operations

## ASYMPTOTIC TYPES
* efficiency functions, most to least efficient:
	- Constant O(1): most efficient. you need to retrieve the first or last item in a data structure -- you know where it is, just get it
	- Logarithmic O(log(n)): divide and conquer: binary search
	- Linear O(n): linear search for an item in a data structure -- worst case you need to iterate over every item
	- Linearithmic O(nlog(n)): sort and then retrieve ("find the second to largest value in an array") <- is this right?
	- Quadratic O(n^2): nested foreach to find matching values
	- Exponential O(2^n) or O(e^n)
	- Factorial O(n!): worst
* big-O can describe space complexity as well as performance complexity
* general identification of asymptotic type:
	- if a list or array gets iterated over once it is most likely O(n) time
	- if half of the elements are visited, it is likely O(log(n)) time
	- if you have singly-nested loops, it is likely quadratic
* [summary of types](https://www.educative.io/courses/data-structures-interviews-cs/qVQq0WLjO3p)

## NORMALIZATION
* normalizing your data before processing it can optimize your algorithm's efficiency
* for example, searching a string for a letter value. you could search for both upper and lower case on each char, or you could convert the string to lowercase then search only lowercase.  you could also remove whitespace, or remove other data that you don't want to consider
* you could also sort the data and then retrieve first/last for min/max
* normalization can also make your algorithmic code more flexible and apply to more varied data sets
* however, normalizing data of course reduces the overall efficiency of your algorithm


## ALGORITHM TYPES

### SLIDING WINDOW
* real world problems:
	- telecommunications: find the max number of users connected to a cell base station in every k-millisecond sliding window
	- streaming: given a number of buffering events in a user session, calculate the median number of buffering events in each one-minute interval


### VALIDATION
* returns true or false


### LINEAR SEARCH
* iterate through each item in a list/array - O(n) performance
* needed when no assumptions/normalization can be made or when the data is always small

### BINARY SEARCH
* "divide and conquer" strategy
* assumption: data must be sorted, generally in a particular order. this can cause a serious hit on efficiency -- it can be more performant to just do a linear search than it is to sort. BUT if you're doing other stuff with your data in which it's helpful to have it sorted, it can still be a good idea overall
* halve your data and search one half or the other depending on which half would have your value
* this algorithm runs in O(log(n)) time vs O(n) time for linear search
* C# Linq has a built in BinarySearch method



## DATA STRUCTURES
* **arrays**
	- used to implement lists, stacks and queues
	- data stored in contiguous location
	- index makes it easy to retrieve first/last etc
	- pro: lower overhead, more efficient than linked lists, trees etc
	- con: not flexible; if you want to add/remove, you'll have to create a new one
* **linked lists**
	- used to implement lists, stacks and queues
	- includes doubly and circular linked lists
	- data is not stored in contiguous location -- consists of nodes which contain data and a pointer to the next node (and in the case of doubly linked lists, a pointer to the prev node)
	- C# contains built in type `LinkedList<T>` so the overhead/implementation is abstracted away
	- `head` and `tail` refer to first and last node
	- pro: these are flexible -- dynamically add or remove items
	- con: less efficient than array, more overhead
* **array of linked lists**
	- used to implement hash table
* **BST**
	- useful for working with non-linear data that is always arranged in order but may need to be flexibly inserted/removed
	- composed of nodes where each node is linked to up to two nodes
	- the `root` node is the starting node. each node has `children` and all nodes with children are `parents`
	- the best worst case efficiency is O(log(n)) on a well-balanced tree since it cuts down traversals, but if your tree is poorly balanced (all elements are on one side) the worst case time can still be O(n)
	- *in-order traversal*: visit left, then parent, then right; ex: print in order
	- *pre-order traversal*: visit parent, then left, then right; ex: look for leaves or search
	- *post-order traversal*: visit left, then right, then parent; ex: deletion


## ABSTRACT DATA TYPES
* **queue**
	- items can only be added to the back and removed from the front. elements stay in the order they were inserted. if your data needs to stay in order and be FIFO, queue is a good choice
	- *FIFO*: first in, first out. items in the middle cannot be accessed, only *head* and *tail*
	- *enqueue* adds an item to the front
	- *dequeue* adds an item to the back
	- *peek* accesses the item at the front
	- pro: O(1) efficiency to retrieve
	- con: only work for FIFO cases
* **stack**
	- similar to the queue, ordering is important -- the only accessible item in this case is the top
	- example: runtime stack tracks the state of a program
	- *FILO*: first in, last out. only *top*
	- *push* an item on the top of the stack
	- *pop* an item off the top
	- *peek* accesses the item at the top
	- pro: O(1) efficiency to retrieve
	- con: only work for FILO cases
* **hash set**
	- if your data is already in a hash set/doesn't need to be ordered, it can be retrieved in constant time (O(1))
	- hash sets are unordered collections of unique items. great for tracking unique items
	- example: you need to check if a vendor code is valid -- you can check in a hash set with O(1) performance
	- no type safety
	- invalid key results in null
	- thread safe
	- pro: constant time retrieval
	- cons: no iteration
* **hash table**
	- stores a collection of key-value pairs based on the hash of the key
	- cannot store duplicate keys, but can store duplicate values
	- keys and values are unordered but can be iterated
	- retrieval is linear O(n) as accessing the key is constant, but if there are multiple values stored for that key, all may need to be iterated to locate the item (and supposing we have a bad hash/unlucky data, all our data could be stored under the same key...)
* **dictionary**
	- set of key-value pairs in a generic collection -- C# built in Dictionary is a hash table under the hood
	- example: you need to retrieve the discounts available for one of many coupons
	- type safe unlike hash tables/sets
	- cannot store duplicate keys, but can store duplicate values
	- keys and values are unordered but can be iterated
	- invalid key results in error
	- thread safe for public static members
	- pro: constant time retrieval


## MISC NOTES
* `for` vs `foreach`
	- `for` is more efficient than `foreach`, particularly on lists
	- iterating on an array is more efficient than iterating on list
	- however you may prefer readability over micro-optimizations -- `foreach` shows very clear intent whereas `for` could be doing lots of stuff
* `string` vs `stringBuilder`
	- strings are easy to use and great, but they're immutable -- any time you modify one, you create a new one. if you're modifying a lot of strings over and over, performance is not great
	- `stringBuilder` is an excellent tool if you need to modify strings and know the target length
* **recursion** is neat and especially helpful for traversing trees, but not always the best choice

## PRACTICE
* an e-commerce cart offers free shipping on orders over $100. if a cart total is less than $100, how would you id other products that would qualify them for free shipping?

