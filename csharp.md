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
    ```


## C# OBJECT ORIENTED PROGRAMMING
* using OOP helps us to accomplish and conform to the following:
    - clean code
    - defensive coding
    - domain driven design
    - design patterns
    - iterative agile


### CLASS STRUCTURE
* **classes** define the structure of **objects** which are instances of the class
* class **members** consist of
    - **methods**
    - **properties**
* an instance of a class is often called an **object variable**
    `Customer customer = new Customer();``
    - objects are transitory -- they do not exists when the code is not running
* **business object** often refers to a class that solves a particular problem (in this case, object == class)
* an **entity** is something from the real world that is being represented by a class
* putting it all together:
    - a _customer_ is something that's important to business, so it is an **entity**
    - we create a `Customer` class to represent our real-life customers
    - we create instances (or **objects**) of the `Customer` class which contain all the class properties

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


### CREATING A BUSINESS LAYER
* in VB select `New Project > Visual C# > Class Library`



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
    Using Customer.BLL
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




### RESOURCES
* Check [msdocs coding conventions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/inside-a-program/coding-conventions)
* [Json.NET Samples](https://www.newtonsoft.com/json/help/html/Samples.htm)
