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


### C# NOTES
* access rights:
    - private: only accessible by the class itself
    - protected: accessible by a class and its children
    - public: accessible by anyone
    - internal/protected internal: accessible only by methods within the same assembly or by derived classes from other assemblies



### RESOURCES
* Check [msdocs coding conventions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/inside-a-program/coding-conventions)
* [Json.NET Samples](https://www.newtonsoft.com/json/help/html/Samples.htm)
