# DESIGN PATTERNS

* **observer**: used when there is a one-to-many relationship between objects such that if one object is modified, its dependent objects are to notified automatically
    - the observer pattern uses three actors: **Subject**, **Observer** and **Client**. The Subject is an object which has methods to attach and detach observers to a client object
    - an observer or observers are attached to the subject and when the subject is modified, its method notifies any attached observers
    - the client is an instance of the subject to which observers are attached (?)
* **repository**: we generally don't want code mucking around with data willy-nilly. repositories are in charge of interacting with databases, performing actions such as adding, removing, updating and selecting items from a collection without worrying about the nuances of the database. see [this website](https://deviq.com/repository-pattern/) for more info
* **factory**: very common creational pattern used to create objects without exposing the creation logic to the client
    - the newly created object is referenced using a common interface -- this requires an inheritance hierarchy, of course
    - example: a ShapeFactory creates a shape of any of several types (circle, square, triangle, etc) and once the shape is created, we just refer to it as `shape2` or whatever -- we don't care what kind it is
* **lazy load**: delaying the actual load of an object until the moment it's needed.
    - typically used when the cost of creating an object is very high and use of the object is rare
    - four common implementations of the Lazy Loading pattern are:
        * Virtual Proxy: delayed creation of objects (like a list). might look something like:
            ```java
                if (contactList == null) {
                    System.out.println("Loading employee data");
                    // here is the actual contact list class but the calling procedure never actually interacts
                    //    with it -- it all goes through proxy
                    contactList = new ContactListImpl();
                }
                return contactList.getEmployeeList();
            ```
        * Lazy Initialization: by comparison to virtual proxy, the lazy initialization technique checks the value of a class field when it needs to be used. if that value is equal to null, then that field gets loaded before being used
        * Ghost: an object that is loaded in a partial state
            - allows use of objects in their partial state
            - however it is more complicated to implement
        * Value Holder: a value holder is a generic object that handles the lazy load behavior and appears in place of an object's data fields
* **decorator**:
* **proxy**:
* **prototype**:         
* **singleton**: an object whose instantiation is restricted to one instance (only one is needed or permitted at a time)
