# CODING IN C#

## SYNTAX
* like C++, the `Main` method is your ticket into an application
    - `Main` is a method which resides in a class or struct
    - `Main` can return an int and take arguments

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


## C# ON THE COMMAND LINE
* to compile a project, run `$ csc Hello.cs`. this will generate an executable of the same name as your file with the `.exe` extension
* then run `$ Hello.exe` to run the program

### USING DOTNET
* Initially I just used the `csc` compiler to build my program but when I needed to pull in the `Newtonsoft.Json` package, I switched over to using `dotnet`
* Using `dotnet`:
    - once installed, you can use `dotnet add package` to install and make packages available.
    - `dotnet` implicitly calls the `nuget` package manager to install packages and maintain dependencies
    - basic `dotnet` commands:
    ```
        dotnet new console          // create a new project with a Program.cs file and main()
        dotnet build                // builds a project and all its dependencies
        dotnet run                  // first builds and then runs the application
        dotnet install package foo  // install package foo and make it available to your code
        dotnet add package CCI.Communications.Client -s https://nuget.altsrc.net/api/v2/ // specify package source
    ```

# Language Basics

## OBJECTS
* an object is a complex reference type
* objects are transitory -- they do not exists when the code is not running
    ```csharp
        // "Object variable customer is a reference pointing to a new instance of the Customer class"
        Customer customer = new Customer();
    ```
* we can use the **var** keyword when declaring an implicitly typed local variable
    * using `var` doesn't mean that it's not strongly typed -- its type is just as defined as if you had used the class name. we can use `var` because the type is obvious

### OBJECT INITIALIZER SYNTAX
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
        Student std = new Student() { StudentID = 1,
                                    StudentName = "Bill",
                                    Age = 20,
                                    Address = "New York"
                                    };
    ```

## DATA
* objects are **reference types**: variables that store references to their properties
    - this means that:
        ```csharp
            var customer1 = new Customer();
            customer.FirstName = "Bilbo";
            var customer2 = customer1;
            customer.2.FirstName = "Frodo";
            Console.log(customer1.FirstName); // "Frodo"
        ```
* ints and other primative types are **value types** store their values directly

## TYPES AND CASTING
* the `is` operator can used to test if an expression result or variable is compatible with a given type. the evaluation of `is` returns a boolean.
* the `as` operator explicitly converts the result of an expression to a given reference or nullable type. the `as` operator never throws an exception (as compared to the **cast operator**)
    - comparing `as` to type casting: `as` is equivalent to `E is T ? (T)(E) : (T)null;` except the value of `E` is only read once
* a `cast expression` takes the format `(T)E` and performs an explicit conversion of the expression `E` to type `T`. If no explicit conversion can be made, it will not compile. At run time, an exception might be thrown.
* the `typeof` operator returns the `System.Type` instance for a type
    - arguments must be the name of a type or a type parameter
    - arguments cannot be an expression. to get the `System.Type` of an expression, use `Object.GetType()`



# C# OBJECT ORIENTED PROGRAMMING
* using OOP helps us to accomplish and conform to the following:
    - clean code
    - defensive coding
    - domain driven design
    - design patterns
    - iterative agile


## CLASSES
* **classes** define the structure of **objects** which are instances of the class
* class **members** consist of
    - **methods**
    - **properties**
* an instance of a class is often called an **object variable**
* **business object** often refers to a class that solves a particular problem (in this case, object == class)
* an **entity** is something from the real world that is being represented by a class
* putting it all together:
    - a _customer_ is something that's important to business, so it is an **entity**
    - we create a `Customer` class to represent our real-life customers
    - we create instances (or **objects**) of the `Customer` class which contain all the class properties


### STATIC MODIFIER
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

### FIELDS AND PROPERTIES AND AUTOPROPERTIES, OH MY!
* a **field** is a private (or protected) class member that stores the actual data
* a **property** allows the field to be accessed, but only exposes the contract
* an **autoproperty** automatically generates a backing field when you define the property

### DATA ACCESS
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


#### ACCESS MODIFIERS
* **private**: only accessible by the class itself
* **protected**: accessible by a class and its children
* **public**: accessible by anyone
* **internal/protected internal**: accessible only by methods within the same assembly or by derived classes from other assemblies




## GENERICS
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

### GENERIC TYPE PARAMETERS
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


## LAYERING
* in addition to breaking the code down into classes, it also makes sense to break the application itself into portable units or **layers**
* layering makes it easier to extend the application
* layers include:
    - **user interface**: forms or pages displayed to the user; logic to control the UI elements (`exe`)
    - **business logic**: logic to perform business operations (`dll`)
    - **data access**: logic to retrieve and save data to and from the database (`dll`)
    - **common**: code that is applicable to many layers and perhaps multiple applications (`dll`)
        - ex: logging, sending an email
* each layer is encapsulated into a separate project


### CREATING A BUSINESS LAYER
* in VB select `New Project > Visual C# > Class Library`
    - name it something like solution: "ACM", project name: "ACM.BL"
    - Application -> Visual Studio Solution
    - Layer Component -> Visual Studio Project

## DOMAIN DRIVEN DESIGN
* programmers tend to think in terms of models -- what is the shape of this data? what does it do? but that doesn't always scale up encorporate the high-level business requirements. thinking about the domain as a whole can help capture the bigger picture
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


# EXCEPTION HANDLING



# LIBRARIES
* a library is code that compiles to a DLL (rather than `exe`) and is used by other projects
* DLLs therefore do not have entry points and cannot be run on their own -- if needed, a console app with an entry point must be created to run the DLL

## DEBUGGING DLLs
* [debugging DLLs](https://docs.microsoft.com/en-us/visualstudio/debugger/debugging-dll-projects?view=vs-2019)
* debugging a class library (or anything that compiles to a DLL) is different than debugging something that compiles to an `exe`


# TESTING
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



# EXTENSIONS

## WHAT ARE THEY?
* extensions are really cool! they are:
    - additional methods that allow you to inject additional methods without modifying, deriving or recompiling the original class, struct or interface
    - extension methods can be added to your own custom class, .NET framework classes, or third party classes or interfaces
    - static
    - available throughout the application by including the namespace in which it has been defined
* The only difference between a regular static method and an extension method is that the first parameter of the extension method specifies the type that it is going to operate on, preceded by the this keyword
* The first parameter of the extension method must be of the type for which the extension method is applicable, preceded by the this keyword

## EXAMPLE
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


# MISC TRICKS AND THINGS
* **string interpolation** provides a more readable and convenient syntax to include formatted expression results in a result string
    ```csharp
        Console.WriteLine($"On {date:dddd, MMMM dd, yyyy} Leonhard Euler introduced the letter e to denote {Math.E:F5} in a letter to Christian Goldbach.");
    ```
* **implicit types** just use `var` -- the compiler knows what to do
* **linq**: Language Integrated Query, a .NET library which allows you to write queries directly into your code
    - when using linq, consider using `list.FirstOrDefault()` over `list.First()` as the former will not throw an exception if there is no element


### LOCKING
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


# RESOURCES
* Check [msdocs coding conventions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/inside-a-program/coding-conventions)
* [Json.NET Samples](https://www.newtonsoft.com/json/help/html/Samples.htm)
