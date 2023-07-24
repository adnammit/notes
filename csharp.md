# C#

## Syntax
* like C++, the `Main` method is your entry point into an application
	- `Main` is a method which resides in a class or struct
	- `Main` is static and can return an int and take arguments

```c
	// comments can be single line
	// here is an example of a void Main():
	static void Main()
	{
		System.Console.WriteLine("Hello World");
	}

	/* comments may also be made
	on multiple lines */
	/* Here we have a Main() which takes args
	and returns a value */
	static int Main(string[] args)
	{
		System.Console.WriteLine(args);
		return 0;
	}

```
* using `System` methods and classes like `Console.WriteLine`
	- in your code include the `using System` directive at the beginning of the program and then you will be able to write `Console.WriteLine` instead of `System.Console.WriteLine`


# Building C# Code
* C# is a compiled language -- it must be compiled before it can be run
* there are basically three options for compiling C# code: csc, dotnet and msbuild
* [some differences between build options](https://stackoverflow.com/a/56949667)
* [how MSBuild builds projects](https://learn.microsoft.com/en-us/visualstudio/msbuild/build-process-overview)

## CSC
* csc is a command line csharp compiler
* most minimal way to build C# code
* targets files or filesystems -- you can specify individual files or directories to compile, but not .NET projects or solutions
* produces exes, dlls or code modules (.netmodule)
* invoked by MSBuild to build .NET Framework projects
* to compile a project, run `$ csc Hello.cs`. this will generate an executable of the same name as your file with the `.exe` extension
* then run `$ Hello.exe` to run the program

## MSBuild
* msbuild is the Microsoft build engine
* msbuild is not a compiler -- it orchestrates other build operations and applies the appropriate build strategy for the project type
* integrated into Visual Studio (when you hit F5) but can be used on the command line for deployment pipeline etc
* compiles at a project or solution level
* can be used to configure environment/build variables, customize build operations (like transpiling and minifying ts/js/css)
* handles all .NET languages including C#, C++, F#, VB.NET, etc

### Visual Studio Build vs MSBuild on the Command Line
* there are some [significant differences between a build in VS and invoking MSBuild directly](https://learn.microsoft.com/en-us/visualstudio/msbuild/build-process-overview?view=vs-2022#visual-studio-builds-vs-msbuildexe-builds)
* VS manages the project build order directly
* when MSBuild is invoked with a sln on the command line, the sln is parsed and executed as a project. with VS, MSBuild never sees the sln file
* therefore solution build customization does not apply to VS builds

## dotnet
* dotnet is somewhere between csc and msbuild -- a command line-only tool that can build projects and solutions
* TODO would csc vs dotnet be the different compilation approaches for unmanaged and managed code?


# Language Basics

## Objects
* an object is a complex reference type
* objects are transitory -- they do not exists when the code is not running
	```csharp
		// "Object variable customer is a reference pointing to a new instance of the Customer class"
		Customer customer = new Customer();
	```
* we can use the **var** keyword when declaring an implicitly typed local variable
	* using `var` doesn't mean that it's not strongly typed -- its type is just as defined as if you had used the class name. we can use `var` because the type is obvious

### Object Initializer Syntax
* for a class with no constructor, you can create a `new` object by using the following curly-brace syntax:
	```csharp
		public class Student
		{
			public int StudentID { get; set; }
			public string StudentName { get; set; }
			public int Age { get; set; }
			public string Address { get; set; }
		}

		// create an instance of Student without invoking a constructor:
		Student std = new Student() {
			StudentID = 1,
			StudentName = "Bill",
			Age = 20,
			Address = "New York"
		};
	```

# Types
* objects are **reference types**: variables that store references to their properties
	- this means that:
		```csharp
			var customer1 = new Customer();
			customer.FirstName = "Bilbo";
			var customer2 = customer1;
			customer.2.FirstName = "Frodo";
			Console.log(customer1.FirstName); // "Frodo"
		```
* ints and other primitive types are **value types** store their values directly
* **pointer types**
* **boxing** and **unboxing** allows conversion between value and reference types
	- boxing converts a value type to an object type. boxing is performed implicitly by the CLR. when the CLR boxes a value type, it wraps the value inside a System.Object instance and stores it on the managed heap
	- unboxing converts an object type to a value type. unboxing is performed explicitly by the programmer
	- boxing and unboxing is generally considered bad practice as it's computationally expensive -- boxing requires a new object to be allocated and constructed. the operation is probably unnecessary
	- example:
	```csharp
		int i = 90;
		object o = i; // implicit boxing; o is a pointer on the heap that points to value 90 on the stack
		i = 92;
		int j = (int)o; // unboxing
		Console.WriteLine(i); // 92
		Console.WriteLine(j); // 90 -- really?
	```

## Pass By Reference Vs Value
* when passing a value into a function, a copy of the value is created in memory and any modifications made to the value in the function will not persist outside the function scope (as those changes were made to the copy)
* when passing by reference, we send a reference to the value into the function -- changes to the value will persist outside the function scope as we use the reference to access the value it's pointing to
* *passing by value is the default for all types* including reference types -- when you pass an object into a function, you're passing a copy of the reference to the object value -- you can modify the object in the function with persisting changes to the object outside the function because the reference copy allowed you to modify the values of the object
* we can use the `ref` keyword to pass value types by reference
* example:
	```csharp
		var foo = new MyObject();
		foo.name = "Carly";
		ChangeObject(foo) => {
			foo.name = "Bob"; // changes name to "Bob" outside function scope
			foo = new MyObject(); // nope, now this ref copy is pointing to a different thing that won't exist outside this func execution
			foo.name = "Bill";
		};
		Console.WriteLine(foo.name); // "Bob"
	```
* the `out` keyword has a use case that is similar to `ref`
	- `ref`: the object is initialized outside the function. the function can read and modify the object
	- `out`: the object will be initialized inside the function. the value must be set before returning. out is one way to cheat and return multiple values from a function
* using `out` and `ref` can be antipatterns and should only be used in select scenarios
	- `ref` can cause side effects -- generally you want to `return` the changes you want to persist
	- `out` shouldn't be used to "cheat" and return multiple values -- you should return an object or tuple that contains all the data you need


## Types And Casting
* the `is` operator can used to test if an expression result or variable is compatible with a given type. the evaluation of `is` returns a boolean.
* the `as` operator explicitly converts the result of an expression to a given reference or nullable type. the `as` operator never throws an exception (as compared to the **cast operator**)
	- comparing `as` to type casting: `as` is equivalent to `E is T ? (T)(E) : (T)null;` except the value of `E` is only read once
* a `cast expression` takes the format `(T)E` and performs an explicit conversion of the expression `E` to type `T`. If no explicit conversion can be made, it will not compile. At run time, an exception might be thrown.
* the `typeof` operator returns the `System.Type` instance for a type
	- arguments must be the name of a type or a type parameter
	- arguments cannot be an expression. to get the `System.Type` of an expression, use `Object.GetType()`

## Strings
* strings are immutable. when we manipulate or concatenate strings, we aren't modifying the original -- we're creating new ones
* methods for concatenation:
	- concatenation operator: `+`
	- StringBuilder: a flexible class which allows us to dynamically construct strings without creating a new one with every modification
```csharp
	string firstName = "Barb";
	string lastName = "Kilner";
	string name = firstName + " " + lastName;

	var builder = new StringBuilder("Hello");
	builder.Append(" World");
	builder.Insert(0, "Ahoy ");
	builder.Remove(4,9);
	string greeting = builder.ToString();
	Console.WriteLine(greeting);
```

## Null
* the default value of reference types is null (string = null)
* normally, value types have defaults that correspond to their type (int = 0, bool = false, etc)
* however a **nullable value type** can be null: `int? i = null`
* as of C#8/.NET6 we have **nullable annotation context**
	- basically an agreement that all your reference types which *can* be null will either be explicitly be assigned a non-null value (never null) or will be marked as nullable with question mark
	- with this feature enabled, IDE warns if you have a reference type that is null that you might not be expecting
		```csharp
		// IDE warns: Converting null literal or possible null value to non-nullable type
		string s = null;
		// this however is happy and should be more evident in later execution that your value could be null:
		string? s = null;
		```
	- enabled by default in .NET6+ -- in the csproj file you'll see something like `<Nullable>enable</Nullable>`


# Classes
* **classes** define the structure of **objects** which are instances of the class
* class **members** consist of
	- **methods**
	- **fields**: variables declared at the class level
	- **properties**: public contract used to access (get/set) and manipulate the field value
* an instance of a class is often called an **object variable**
* **business object** often refers to a class that solves a particular problem (in this case, object == class)
* an **entity** is something from the real world that is being represented by a class
* putting it all together:
	- a _customer_ is something that's important to business, so it is an **entity**
	- we create a `Customer` class to represent our real-life customers
	- we create instances (or **objects**) of the `Customer` class which contain all the class properties


## Access Modifiers
* **private**: only accessible by the class itself
* **protected**: accessible by a class and its children
* **public**: accessible by anyone
* **internal/protected internal**: accessible only by methods within the same assembly or by derived classes from other assemblies

## Data Access
* a class encapsulates its data and access to that data is controlled via `getters` and `setters`
* `getters` and `setters` map to **backing fields** -- the actual data
* you could manually create a property that returns or sets the value of the backing field, as in the first example below. but you only want to do this if there's some logic you need to perform before getting or setting the data. Otherwise, use **auto-implemented properties**
	```csharp
		// full property syntax with a backing field:
		public class Customer
		{
			private string _lastName;
			public string LastName
			{
				get {
					// some code
					return _lastName;
				}
				set {
					// some code
					_lastName = value;
				}
			}
		}

		// or more simply, using auto-implemented properties:
		public class Customer
		{
			// this implements the backing field as well
			public string FirstName { get; set; }
			public string LastName { get; set; }
		}
	````
* example of class creation:
	- note that we don't want anything outside our `Customer` class setting the `CustomerId` class so the `set` is private
	- `FullName` is calcualted from `LastName` and `FirstName` and you don't need to use our private backing fields -- just use the ones we defined
	- you can create a **"read only"** property by only defining the `getter` (as with `FullName`)

	```csharp
		public class Customer {
			public string FirstName { get; set; }
			public string EmailAddress { get; set; }
			public int CustomerId { get; private set; }
			public string FullName {
				get {
					return LastName + ", " + FirstName;
				}
			}
		}

	```

## Fields And Properties And Autoproperties, Oh My!
* a **field** is a private (or protected) class member that stores the actual data
* a **property** allows the field to be accessed, but only exposes the contract
* an **autoproperty** automatically generates a backing field when you define the property


## Static Modifier
* entire classes can be static, and individual properties/methods of a nonstatic class can be static
* the **static modifier** declares a member that belongs to the class itself -- rather than to an object of a class
	- it is accessed using the class name
	- it is not an object variable
	```csharp
		public static int InstanceCount { get; set; }
		// later, in code:
		Customer.InstanceCount += 1;
		// not this:
		var customer = new Customer();
		customer.InstanceCount = count; // nope
	```
* static classes cannot be instantiated or inherited and all members must be static

## Const And Readonly
* **const** keyword creates a constant variable:
	- const variables can be local or a class member
	- const variables must be assigned a value when they are declared -- they are immutable after creation
	- const variables can be initialized using other const values as long as it doesn't create a circular reference
	- const can only be used to to store built-in types, not Object, classes, arrays or structs
	- const values are static even without the `static` keyword: they are accessed like static fields because the value will be the same for every instance of the class
* **readonly** is used to create a class, struct or array that is initialized one time at runtime (ex: in a constructor) and cannot be changed thereafter
	- readonly cannot be used on local variables -- only on class fields
	- readonly fields can be assigned to in the declaration or in a constructor
	- readonly fields can therefore have variable values depending on how the value is initialized at runtime
	- when an object is readonly, the immutability only applies to the reference -- you cannot reassign the field to a different object, but you can modify the subproperties of the object
	```csharp
	public class Calendar
	{
		public const int Months = 12;
		public const int Weeks = 52;
		public const int Days = 365;
		public const double DaysPerWeek = (double) Days / (double) Weeks;
		public const double DaysPerMonth = (double) Days / (double) Months;
		public readonly int HoursPerDay = 24;
		public Calendar(int hours)
		{
			HoursPerDay = hours;
		}

		// accessing a constant outside the class like a static field:
		int birthstones = Calendar.Months;
	}
	```


# Generics
* csharp 2.0 and up, you can create a class with a placeholder type that is assigned at compile time
```csharp
	// class definition:
	class MyGenericClass<T>
	{
		private T genericMemberVariable;

		public MyGenericClass(T value)
		{
			genericMemberVariable = value;
		}

		public T genericMethod(T genericParameter)
		{
			Console.WriteLine("Parameter type: {0}, value: {1}", typeof(T).ToString(),genericParameter);
			Console.WriteLine("Return type: {0}, value: {1}", typeof(T).ToString(), genericMemberVariable);

			return genericMemberVariable;
		}

		public T genericProperty { get; set; }
	}

	// instantiate the generic class:
	MyGenericClass<int> intGenericClass = new MyGenericClass<int>(10);

	int val = intGenericClass.genericMethod(200);

```

## Generic Type Parameters
* a type parameter is a placeholder for a specific type that a client specifies when they create an instance of the generic type
* A generic class, such as GenericList<T> listed in Introduction to Generics, cannot be used as-is because it is not really a type; it is more like a blueprint for a type
* To use GenericList<T>, client code must declare and instantiate a constructed type by specifying a type argument inside the angle brackets
* The type argument for this particular class can be any type recognized by the compiler
```csharp
T GetDefault<T>()
{
	return default(T);
}
```
Note that the return type is T. With this method you can get the default value of any type by calling the method as:
	```csharp
	GetDefault<int>(); // 0
	GetDefault<string>(); // null
	GetDefault<DateTime>(); // 01/01/0001 00:00:00
	GetDefault<TimeSpan>(); // 00:00:00
	```
.NET uses generics in collections, ... example:
```csharp
List<int> integerList = new List<int>();
```
This way you will have a list that only accepts integers, because the class is instancited with the type T, in this case int, and the method that add elements is written as:
```csharp
public class List<T> : ...
{
	public void Add(T item);
}
```
Some more information about generics.

You can limit the scope of the type T.

The following example only allows you to invoke the method with types that are classes:

```csharp
void Foo<T>(T item) where T: class
{
}
```
The following example only allows you to invoke the method with types that are Circle or inherit from it.

```csharp
void Foo<T>(T item) where T: Circle
{
}
```
And there is new() that says you can create an instance of T if it has a parameterless constructor. In the following example T will be treated as Circle, you get intellisense...

```csharp
void Foo<T>(T item) where T: Circle, new()
{
	T newCircle = new T();
}
```
As T is a type parameter, you can get the object Type from it. With the Type you can use reflection...

```csharp
void Foo<T>(T item) where T: class
{
	Type type = typeof(T);
}
```
As a more complex example, check the signature of ToDictionary or any other Linq method.

```csharp
public static Dictionary<TKey, TSource> ToDictionary<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector);
```
There isn't a T, however there is TKey and TSource. It is recommended that you always name type parameters with the prefix T as shown above.


# Layering
* in addition to breaking the code down into classes, it also makes sense to break the application itself into portable units or **layers**
* layering makes it easier to extend the application
* layers include:
	- **user interface**: forms or pages displayed to the user; logic to control the UI elements (`exe`)
	- **business logic**: logic to perform business operations (`dll`)
	- **data access**: logic to retrieve and save data to and from the database (`dll`)
	- **common**: code that is applicable to many layers and perhaps multiple applications (`dll`)
		- ex: logging, sending an email
* each layer is encapsulated into a separate project


## Creating A Business Layer
* in VB select `New Project > Visual C# > Class Library`
	- name it something like solution: "ACM", project name: "ACM.BLL"
	- Application -> Visual Studio Solution
	- Layer Component -> Visual Studio Project


# C# Object Oriented Programming
* using OOP helps us to accomplish and conform to the following:
	- clean code
	- defensive coding
	- domain driven design
	- design patterns
	- iterative agile

# Domain Driven Design
* programmers tend to think in terms of models -- what is the shape of this data? what does it do? but that doesn't always scale up incorporate the high-level business requirements. thinking about the domain as a whole can help capture the bigger picture
* DDD still depends on SRP (Single Responsibility Principal)
* Anti-Corruption Layers (ACL) are another key DDD pattern
	- ACLs are essentially seams that prevent non-domain concepts from leaking into your model
	- repos are another kind of seam
* **entities** are "things" in your system: person, place, thing
	- entities have an identity and a lifecycle -- when the process is complete, the entity is dead as far as the system is concerned and goes back to long-term storage
	- an entity is a unit of behavior: when you invoke a command on an entity, it is responsible for changing its internal state
	- use **read-only** to enforce immutability of entities
* core behavior should be owned by the entity but you might need to provide it with additional data, or provide dependencies which will be responsible for imposing side effects on the outside world
	```csharp
		public class Policy { public void Renew(IAuditNotifier notifier) { // do a bunch of internal state-related things, // some validation, etc. ... // now notify the audit system that there's // a new policy period that needs auditing notifier.ScheduleAuditFor(this); } }
	```


# Exception Handling

# Locks
* the `lock` statement is an integrated shorthand for restricting access to a block of code to only one thread at a time
* the `lock` construct very simply requires that a `reference_type` object be instantiated as the lock (i.e., you cannot use a `value_type` such as an int as a lock)
* For example, if you have an `Account` class with `Debit` and `Credit` methods, you can lock the balance to make sure a deposit is not being made at the same time as a withdrawal:
```csharp
public decimal Debit(decimal amount)
{
	lock (balanceLock)
	{
		if (balance >= amount)
		{
			Console.WriteLine($"Balance before debit :{balance, 5}");
			Console.WriteLine($"Amount to remove     :{amount, 5}");
			balance = balance - amount;
			Console.WriteLine($"Balance after debit  :{balance, 5}");
			return amount;
		}
		else
		{
			// note: this is kind of stupid imho -- should probably throw an error
			return 0;
		}
	}
}
```



# Libraries
* a library is code that compiles to a DLL (rather than `exe`) and is used by other projects
* DLLs therefore do not have entry points and cannot be run on their own -- if needed, a console app with an entry point must be created to run the DLL

## Debugging DLLs
* [debugging DLLs](https://docs.microsoft.com/en-us/visualstudio/debugger/debugging-dll-projects?view=vs-2019)
* debugging a class library (or anything that compiles to a DLL) is different than debugging something that compiles to an `exe`


# Testing
* if we're working with layered design, how do we test a dll?
* we write an **automated** code test -- a separate body of code that's sole purpose is to test another piece of code
* tests usually have the same name as the code it's testing with the `Test` suffix
* create the test through VS by creating a new project of type test and
* steps for building an automated test:
	- **arrange**:  any preparations such as creating an instance of the subject class, and defining the **expected** result
	- **act**: perform an action that returns an **actual** value
	- **assert**: evaluate the actual value as compared to the expected value
	```csharp
	Using Customer.BLL // reference the class you're testing
		[TestClass]
		public class CustomerTest
		{
			[TestMethod]
			public void FullNameTestValid()
			{
				// --- arrange
				Customer customer = new Customer();
				customer.FirstName = "Bilbo";
				customer.LastName = "Baggins";
				string expected = "Baggins, Bilbo";

				// --- act
				string actual = customer.FullName;

				// --- assert
				Assert.AreEqual(expected, actual);

			}
		}
	```
* pinning Test Explorer might be useful
	- you can "Run All"
	- or right-click and "run test" or "debug run"



# Extensions

## What Are They?
* extensions are really cool! they are:
	- additional methods that allow you to inject additional methods without modifying, deriving or recompiling the original class, struct or interface
	- extension methods can be added to your own custom class, .NET framework classes, or third party classes or interfaces
	- they are static
	- available throughout the application by including the namespace in which it has been defined
* The only difference between a regular static method and an extension method is that the first parameter of the extension method specifies the type that it is going to operate on, preceded by the this keyword
* The first parameter of the extension method must be of the type for which the extension method is applicable, preceded by the this keyword

## Example
```csharp
// Extensions.cs
namespace ExtensionMethods
{
	public static class IntExtensions
	 {
		public static bool IsGreaterThan(this int i, int value)
		{
			return i > value;
		}
	}
}

// Program.cs
using ExtensionMethods;

class Program
{
	static void Main(string[] args)
	{
		int i = 10;
		bool result = i.IsGreaterThan(100);
		Console.WriteLine(result);
	}
}
```


# Misc Tricks And Things
* **string interpolation** provides a more readable and convenient syntax to include formatted expression results in a result string
	```csharp
		Console.WriteLine($"On {date:dddd, MMMM dd, yyyy} Leonhard Euler introduced the letter e to denote {Math.E:F5} in a letter to Christian Goldbach.");
	```
* **implicit types** just use `var` -- the compiler knows what to do
* **linq**: Language Integrated Query, a .NET library which allows you to write queries directly into your code
	- when using linq, consider using `list.FirstOrDefault()` over `list.First()` as the former will not throw an exception if there is no element
* **StringBuilder**: once instantiated, strings are immutable. string concatenation creates new strings. if you're doing a bunch of concatenation, use StringBuilder


# Resources
* Check [msdocs coding conventions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/inside-a-program/coding-conventions)
* [Json.NET Samples](https://www.newtonsoft.com/json/help/html/Samples.htm)
