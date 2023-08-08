# Object Oriented Programming
* [MS documentation](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/tutorials/oop)
* this is pretty geared towards C# but the concepts are fairly universal

## Why OOP?
* using OOP helps us to accomplish and conform to the following:
	- clean, DRY code
	- defensive coding
	- domain driven design
	- design patterns
	- iterative agile
* having encapsulated classes means we can more easily mock testing conditions and make alterations to an object without impacting code outside the object

## Principles of OOP

### Abstraction
* **abstraction**: relevant attributes and interactions of system components are abstractly modeled as classes
* if we're writing banking software, rather than programming all the functions that will happen at the bank, we can focus on the individual entities (customers, accounts, transactions) and code them and test them one at a time

### Encapsulation
* **encapsulation** is the process of hiding the internal workings of an object and exposing only the functionality that is necessary to use it
* encapsulation is achieved by using **access modifiers** to control access to the members of a class: `public`, `private`, `protected`, `internal`, `protected internal`
* class **properties** are used to expose the internal state of an object in a controlled manner
* class **methods** are used to expose the internal behavior of an object in a controlled manner

### Inheritance
* **inheritance**: we can create new abstractions based on existing abstractions
* inheritance is achieved by using **derived classes** that inherit from **base classes** -- the derived class gets all the base class's functionality and can expand on it
* base classes are also sometimes known as **superclasses** or **parent classes**
* derived classes are also sometimes known as **subclasses** or **child classes**

### Polymorphism
* **polymorphism**: we can use a single interface to access a wide range of related classes
* for example, we may have the base class `BankAccount` that is inherited by `CheckingAccount` and `SavingsAccount`
* polymorphism is achieved by using **virtual methods**, **abstract methods** and **abstract classes** to determine which base/derived class functionality will be expressed in a given class instance

## Implementing OOP

### Abstraction
* [MS documentation and examples](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/tutorials/oop)
* when a property is implemented in both the base and derived classes, keywords are used to determine which behavior will take place. the base class may provide either a default for that property that can be overridden in the base class, or it may provide a template that must be implemented in the derived class
* **virtual**: the derived class *may* provide its own implementation
	- **virtual properties** are properties in the base class that *may* be overridden in derived classes
* **abstract**: the derived class *must* provide its own implementation
	- **abstract properties** are properties in the base class that *must* be overridden in derived classes
	- **abstract classes** cannot be instantiated directly -- they must be inherited by a derived class
	- abstract classes may contain both abstract and non-abstract methods
* **pure abstract classes** contain only abstract member functions and no data -- everything must be implemented by the derived class
* when you hear **pure virtual** that generally comes from C++ essentially means "abstract" in C#
* the **override** keyword is used by the derived class to override the base class's virtual or abstract property
* **sealed override** will prevent an additional derived class from overriding further

```csharp
// abstract class with mix of abstract and non-abstract methods
public abstract class Shape {
	public int Id { get; }
	public abstract decimal Area { get; }

	public Shape(int id) {
		Id = id;
	}
}

// derived class
public class Circle : Shape {
	public decimal Radius { get; }
	public override decimal Area {
		get {
			return Radius * Radius * Math.PI;
		}
	}

	public Circle(int id, decimal radius) : base(id) {
		Radius = radius;
	}
}

// pure abstract class
public abstract class Animal {
	public abstract string Name { get; set; }
	public abstract string GetSound();
}
```


