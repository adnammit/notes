# Design Patterns

## 'The Book' by the Gang of Four (GoF)
* [Design Patterns](file:///C:/Users/pangolin/Dropbox/code%20and%20nerd/codedocs/Design%20Patterns.pdf)

## Creational Patterns

### Factory
* **factory**: very common creational pattern used to create objects without exposing the creation logic to the client
* the newly created object is referenced using a common interface -- this requires an inheritance hierarchy, of course
* example: a ShapeFactory creates a shape of any of several types (circle, square, triangle, etc) and once the shape is created, we just refer to it as `shape2` or whatever -- we don't care what kind it is
* supports dependency inversion as dependents don't have to create instances of their dependencies
* using a factory can also ensure that the objects it's instantiating are all in a valid, complete state
* this also allows for standardization or a set state across environment or situation, e.g. a PersonFactory and a MockPersonFactory both have the same interfaces and return objects of the same shape, with the latter optimized for unit tests


### Prototype
* **prototype**: specify the kinds of objects to create using a prototypical instance and then create new objects by copying this prototype
	- example: we have a graphic editor that can be used to draw out sheet music
	- the editor framework doesn't know anything about notes and clefs etc so how can it be expected to draw them?
	- we create a class named `Graphic` (with methods `Draw(position)` and `Clone()`) and the editor will be able to manipulate, clone and insert instances of `Graphic`
	- classes like `Staff` and `MusicalNote` inherit from the prototype `Graphic`

### Singleton
* [Singleton](https://csharpindepth.com/Articles/Singleton)
* **singleton**: an object whose instantiation is restricted to one instance (only one is needed or permitted at a time) and provide a global instance of it
* advantages to the singleton pattern:
	- manages concurrent access to a single shared resource
	- can be lazy-loaded
* use cases for the singleton pattern:
	- logger class: various clients write messages to a single file
	- configuration class: logic to load and calculate configuration is centralized
	- cache class: used to cache data that is frequently accessed and rarely modified
* the singleton pattern is supported by ASP.NET Core dependency injection: dependencies can be registered on startup as singleton services

#### Implementation
* singletons do not allow params to be specified when an instance is created -- a second request for the obj with different params could result in a different singleton - yikes
* singleton implementation characteristics include:
	- single constructor which is parameter-less and private -- other classes cannot instantiate it or influence its state. private also prevents subclassing (and new instances of those subclasses which violates the pattern)
	- the class is sealed. not strictly necessary in addition to the first point, but it might help the JIT to optimize more
	- a static variable which holds a ref to the single created instance, if any (see: lazy-loading)
	- a public static means of getting the ref to the single created instance, creating one if necessary (see again lazy-loading). this means could be a static var or a static method -- both are thread-safe
* there are different ways of implementing the singleton pattern, from simple/non-thread-safe to the safest and most elegant of which is lazy-loaded and thread-safe
* here is a simple (BAD!) singleton
	```csharp
	public sealed class Singleton
	{
		private static Singleton instance = null;

		private Singleton()
		{ }

		public static Singleton Instance
		{
			get
			{
				// two threads can evaluate this and each create their own instance, breaking the pattern
				if (instance == null)
				{
					instance = new Singleton();
				}
				return instance;
			}
		}
	}
	```
* simple thread-save lazy-load implementation. each thread takes a lock on a shared object and then checks if the instance has been created before creating it. this takes care of the memory barrier between threads (as locking makes sure that all reads occur logically after lock acquisition and unlocking makes sure that all writes occur logically before lock release). this implementation does cause a performance hit as due to the blocking behavior of locks
	```csharp
	public sealed class Singleton
	{
		private static Singleton instance = null;
		private static readonly object padlock = new object();

		Singleton()
		{ }

		public static Singleton Instance
		{
			get
			{
				lock (padlock)
				{
					if (instance == null)
					{
						instance = new Singleton();
					}
					return instance;
				}
			}
		}
	}
	```
* in this implementation, the singleton class contains a nested subclass that returns an instance of the singleton. this provides performance benefits while ensuring lazy loading -- because the nested subclass is static, instantiation of Singleton only occurs on the first reference to the static member of the nested class, which only occurs in Instance
	```csharp
	public sealed class Singleton
	{
		private Singleton()
		{ }

		public static Singleton Instance { get { return Nested.instance; } }

		private class Nested
		{
			// Explicit static constructor to tell C# compiler
			// not to mark type as beforefieldinit
			static Nested()
			{ }

			// internal to be accessed by Singleton.Instance
			internal static readonly Singleton instance = new Singleton();
		}
	}
	```
* preferred Singleton implementation for .NET 4 and higher using **System.Lazy<T>** type. a delegate passed in to the constructor calls the singleton constructor
	```csharp
	public sealed class Singleton
	{
		private static readonly Lazy<Singleton> lazy =
			new Lazy<Singleton>(() => new Singleton());

		public static Singleton Instance { get { return lazy.Value; } }

		private Singleton()
		{ }
	}
	```







## Structural Patterns

### Adapter
### Bridge

### Composite

### Decorator
* **decorator**: inheritance pattern in which a subclass implements characteristics that can be added to an individual object dynamically without affecting the behavior of other objects from the same class.
	- for example, you might have a `visualComponent` class that implements method `draw()`
	- other decorator classes inherit from this class, like `ScrollDecorator` and `BorderDecorator` that implement other attributes (like `borderWidth` and `scrollPosition`).
	- decorators might inherit from a shared `Decorator` mid-level class that inherits directly from the base component class


### Facade

### Flyweight

### Proxy
* **proxy**: provides a surrogate or placeholder for another object to control access to it
* related to lazy loading
* defer the full cost of an object's creation until we actually need to use it
* example: on loading a document, `ImageProxy` objects may be used in place of the actual images until (and if) the page is scrolled down far enough that they're visible



## Behavioral Patterns

### Chain of Responsibility

### Command

### Interpreter

### Iterator

### Mediator

### Memento

### Observer
* **observer**: used when there is a one-to-many relationship between objects such that if one object is modified, its dependent objects are to notified automatically
	- the observer pattern uses three actors: **Subject**, **Observer** and **Client**. The Subject is an object which has methods to attach and detach observers to a client object
	- an observer or observers are attached to the subject and when the subject is modified, its method notifies any attached observers
	- the client is an instance of the subject to which observers are attached (?)

### State

### Strategy

### Template Method

### Visitor



## Other?

### Repository
* **repository**: we generally don't want code mucking around with data willy-nilly. repositories are in charge of interacting with databases, performing actions such as adding, removing, updating and selecting items from a collection without worrying about the nuances of the database. see [this website](https://deviq.com/repository-pattern/) for more info

### Lazy Load
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


