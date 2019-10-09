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


## C# OBJECT ORIENTED PROGRAMMING
* using OOP helps us to accomplish and conform to the following:
    - clean code
    - defensive coding
    - domain driven design
    - design patterns
    - iterative agile


### CLASSES
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

#### STATIC MODIFIER    
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

### GENERICS
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
            // or more simply
            public string FirstName { get; set; }
        }

        // or more simply, using auto-implemented properties:
        public class Customer
        {
            // this implements the backing field as well
            public string FirstName { get; set; }
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


### LAYERING
* in addition to breaking the code down into classes, it also makes sense to break the application itself into portable units or **layers**
* layering makes it easier to extend the application
* layers include:
    - **user interface**: forms or pages displayed to the user; logic to control the UI elements (`exe`)
    - **business logic**: logic to perform business operations (`dll`)
    - **data access**: logic to retrieve and save data to and from the database (`dll`)
    - **common**: code that is applicable to many layers and perhaps multiple applications (`dll`)
        - ex: logging, sending an email
* each layer is encapsulated into a separate project


#### CREATING A BUSINESS LAYER
* in VB select `New Project > Visual C# > Class Library`
    - name it something like solution: "ACM", project name: "ACM.BL"
    - Application -> Visual Studio Solution
    - Layer Component -> Visual Studio Project


## TESTING
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





## LIBRARIES
* **linq**: Language Integrated Query, a .NET library which allows you to write queries directly into your code
    - when using linq, consider using `list.FirstOrDefault()` over `list.First()` as the former will not throw an exception if there is no element


## MISC TRICKS AND THINGS
* **string interpolation** provides a more readable and convenient syntax to include formatted expression results in a result string
    ```csharp
        Console.WriteLine($"On {date:dddd, MMMM dd, yyyy} Leonhard Euler introduced the letter e to denote {Math.E:F5} in a letter to Christian Goldbach.");
    ```
* **implicit types** just use `var` -- the compiler knows what to do    


### RESOURCES
* Check [msdocs coding conventions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/inside-a-program/coding-conventions)
* [Json.NET Samples](https://www.newtonsoft.com/json/help/html/Samples.htm)
