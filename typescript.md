# TYPESCRIPT

## WHAT THE HEY IS IT
* ts is a superset of js that compiles to js
* it is cross-brower and cross-platform compatible
* it's open source
* tooling and support
    - can be used directly with node.js ( )
        * `npm install -g typescript` will allow you to run `tsc foo.ts`

    - VS
        * autocompiles on save
        * adding a new ts file generates a new module with a sample class and interface
        * VS extension "Web Essentials" includes a preview of your js
    - typescript playground allows you to type ts and see the generated js immediately with intellisense
* use the playground and check out docs on [typescript lang](www.typescriptlang.org)

### TS VS JS
* js can feel messy, which leads to difficulty maintaining and structuring
* there is overhead, so you don't always need to bother with it, but it's scalable and good for enterprise solutions
* js provides a dynamic type system
    - that's nice -- variables can hold any object, types are determined on the fly
    - however it's difficult to ensure proper types are being passed
    - using ts enforces better practices (don't have to worry that everyone used `===`)
* devs migrating from server to client side apps can handle ts better
* ts supports interface definitions, which js does not natively support

### FEATURES
* support for js -- you don't have to rewrite everything, you can mix-and-match
* static typing allows you to catch errors during compilation
* encapsulation through classes and modules
* support for construtors, properties and functions (the usual)
* lambda funcs
* intellisense support

### COMPILER
* you can use the command line tool `tsc` to manually compile ts
* visual studio automatically generates a corresponding `.js` file when you save a `.ts` file
* so this nice legible typescript:
    ```typescript
        class Greeter {
            greeting: string;
            constructor (message: string) {
                this.greeting = message;
            }
            greet() {
                return "Hello, " + this.greeting;
            }
        }
    ```
* gets compiled into this javascript:
    ```javascript
        var Greeter = (function () {
            function Greeter(message) {
                this.greeting = message;
            }
            Greeter.prototype.greet = function () {
                return "Hello, " + this.greeting;
            };
            return Greeter;
        })
    ```

### SYNTAX AND KEYWORDS
* ts follows the same basic syntax rules as js
    - {} defines code blocks
    - semicolons end code expressions
    - js keywords like `for`, `if` etc
* keywords
    - `class` - container for members
    - `constructor`
    - `function` - do not use the keyword inside classes, but do use it outside
    - `exports` - export a member from a module
    - `extends` - extend a class or interface
    - `implements` - implement an interface
    - `interface` - contract that can be implemented by other types
    - `module`/`namespace` - container for classes and other scoped code
    - `public`/`private` - member visibility modifiers
    - `...` - rest parameter syntax
    - `=>` - callback functions, return type of a function
    - `<typename>` - casting/conversion
    - `:` - separator between variable/parameter names and types
* hierarchy
    - use of namespaces, modules and classes removes things from the global namespace
    - a **module** is a container that exports code. it contains both classes and interfaces
    - **class**/**interface**
        * fields, constructors/properties/functions



## TYPING, VARIABLES AND FUNCTIONS
* when using the `var` keyword, typescript uses **type inference** (like js and csharp) wherein it infers the type of the variable based on the assignment
* **type annotation** can be used to explicitly set the type
    ```typescript
        // using type inference:
        var num = 2;
        // using type annotation:
        var num: number = 2;
        // ambiguous type, no value
        var foo;
        // var with no value, but we know the type
        var foo: number;
        // type inference with two different types (casts number to string)
        var bar = num + ' is my favorite number.';
        // nope
        var notHappy: number = num + ' is unlucky.';
    ```
* `any` is the base type in typescript -- the default type of a `var` when the type is ambiguous

## CLASSES AND INTERFACES

## MODULES



## TROUBLESHOOTING AND MISC

### TYPESCRIPT IN THE BROWSER
* is your `ts` file not refreshing when you're using chrome devtools?
    - with devtools open, do a `Empty Cache and Hard Reload` by right-clicking on the reload button
