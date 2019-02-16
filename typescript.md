# TYPESCRIPT

## WHAT THE HEY IS IT
* ts is a superset of js that compiles to js
* it is cross-brower and cross-platform compatible
* it's open source

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
* so this:
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
* turns into this:
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


## TYPING, VARIABLES AND FUNCTIONS

## CLASSES AND INTERFACES

## MODULES



## TROUBLESHOOTING AND MISC

### TYPESCRIPT IN THE BROWSER
* is your `ts` file not refreshing when you're using chrome devtools?
    - with devtools open, do a `Empty Cache and Hard Reload` by right-clicking on the reload button
