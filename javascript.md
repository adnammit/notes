
## SYNTACTIC BASICS    
### VARIABLES
* always declare your variables, but we'll talk about undeclared variables anyway.
* undeclared variables do not use the `var` or `let` keyword
    - undeclared variables are always global
    ```javascript
        function foo() {
            y = 1; // throws an error in strict mode
            var z = 2;
        }
        foo();
        console.log(y); // 1
        console.log(z); // throws reference error
    ```
* variables declared with `var` keyword:
    - `var` declares a variable, optionally assigning it to a value
    - default value is `undefined`
    - re-declaring a variable does not cause it to lose its value (unless you also assign it a value)
    - scope is set to "execution context"
        * if a var is declared within a function, its scope is that function
        * if a var is declared outside of a function, its scope is global
* `let` keyword:
    - declares a block-scope variable, optionally assigning it to a value
    ```javascript
        let x = 10;
        if(true) {
            let x = 20;
            console.log(x); //20
        }
        console.log(x); //10
    ```
    - does not create a property on the global object
    ```javascript
        var x = 'hello';
        let y = 'world';
        console.log(this.x); //'global'
        console.log(this.y); //undefined
    ```
* differences between variable types:
    - declared variables are created before any code is executed
    - undeclared variables do not exist until the code assigning them is executed
    - declared variables are non-configurable properties of their execution context and thus, cannot be deleted
    - undeclared variables are configurable and can be deleted
* variable hoisting:
    - because declared variables are processed before any code is executed, declaring them anywhere in code is equivalent to declaring them at the top of the function or script.
    - hoisting only affects the variable's instantiation, not its value assignment, which will occur when the line of code where value assignment takes place is reached.
    - to be as clear as possible about the scope of variables, you should declare all variables at the top of the code block.
    ```javascript
        function do_the_thing() {
            console.log(bar); // undefined
            var bar = 12;
            console.log(bar); // 12
        }
        // this is understood as:
        function do_the_thing() {
            var bar;
            console.log(bar); // undefined
            bar = 12;
            console.log(bar); // 12
        }
    ```



### OBJECTS AND PROPERTIES
* objects can be declared using bracket notation:
    ```javascript
        var person = {
            'firstname': "Jennifer",
            'lastname': "Rodriguez",
            'age': "42",
        };
        // or all in one line:
        var person = { 'firstname':'Amelia', 'lastname':'Pond', 'age':'8' };
        console.log(person.firstname+" is "+person.age); //'Amelia is 8'
    ```
* objects can also be declared using bracket notation:
    ```javascript
        var person = {};
        person['firstname'] = "Jeremy";
        person['lastname'] = "Small";
        person['age'] = "58";
        console.log(person['firstname']+" is "+person['age']); //'Jeremy is 58'
    ```

## MEMORY
### pass by reference vs pass by value
* in js, primitive types will be passed by value and non-primitives will be passed by reference
    ```javascript
        function increment(number) {
            number+=1;
        }
        var number = 6;
        increment(number);
        console.log(number); // 6

        function addStuff(arr,obj) {
            arr.push(5);
            obj.food = 'apple pie';
        }
        var arr = [1,2,3,4];
        var obj = {name: 'Tony', food: 'pizza'};
        addStuff(arr,obj);
        console.log(arr); // [1,2,3,4,5]
        console.log(obj); // {name: 'Tony', food: 'apple pie'}
    ```
* passing by reference is nice b/c you can manipulate multiple objects without returning multiple objects, however this is not cohesive with the "no-side effect" way of functional programming in which a function should not manipulate the external state (by changing values outside of itself)
* passing by reference can be broken if a whole new value is assigned to the variable
    - when this happens, the original pointer points to the old value and a new pointer points to the new data location, so when you return out of your function, the original pointer is still pointing to the old value
    - if you want to keep your reference to the original object, you must manipulate individual elements of that object rather than replacing it entirely
    - for arrays, using methods which manipulate the array in-place (`push`, `push`, `splice`) will preserve your original reference, while methods like `concat`, `slice`, `map` and `filter` will return a new array
    ```javascript
        function changeStuff(arr, obj) {
            arr = [4,5,6];
            obj = {name: "Charlie"};
        }
        var arr = [1,2,3];
        var obj = {name: "Julie", age: "12"};
        changeStuff(arr, obj);
        console.log(arr); // [1,2,3]
        console.log(obj); // {name: "Julie", age: "12"};

        function reallyChangeStuff(arr, obj) {
            arr[0] = 4;
            arr[1] = 5;
            arr[2] = 6;
            obj[name] = "Charlie";
        }
        reallyChangeStuff(arr, obj);
        console.log(arr); // [4,5,6]
        console.log(obj); // {name: "Charlie", age: "12"};
    ```


## STRING METHODS
* none of these things seem to actually alter the value of `str` -- they just return values which you could of course store as variables or use to alter the value of `str`

* index of chars:
    ```javascript
    var str = "Hello";

    str.charAt(0); //H
    str.charAt(4); //o
    ```

* using the unicode of a char
    ```javascript
    var str = "Hello world";

    str.charCodeAt(0); //H == 72
    str.charCodeAt(6); //w == 119

    String.fromCharCode( str.charCodeAt(0) + 1 ); // uses the generic String library to return I
    ```    

* upper and lower case
    ```javascript
    var str = "Hello world";

    str.toUpperCase(); // HELLO WORLD
    str.charAt(6).toUpperCase(); // Hello World
    ```    

* splitting strings
    ```javascript
    var str = "Hello planet Earth";

    var arr1 = str.split(""); // H, e, l, l, o, " ", p, etc
    var arr2 = str.split(" "); // "Hello", "planet", "Earth"
    ```    

* character replacement
    - `replace()` only works on the first instance -- for more mileage, use regex
    ```javascript
    var str = "Hello planet Earth";

    str.replace("Earth","Mars"); // "Hello planet Mars"
    str.replace("e","a"); // Hallo planet Earth -- only replaces the first instance!
    str.split("e").join("a"); // "Hallo planat Earth" -- hacky but it works!

    var str2 = "Duck Duck Goose";
    str2.split("Duck").join("Pheasant"); // "Pheasant Pheasant Goose"
    ```    

* substring
    - the two args sent to `substr()` are the index where the substring starts and the length of the substring
    ```javascript
    var str = "Hello planet Earth";

    var part1 = str.substr(6,6); // "planet"
    str.substr(6,6).toUpperCase(); // "Hello PLANET Earth"
    ```    
    - you can also use the `substring()` method which takes two args for the start of the substring and the ending index of the substring
## REGULAR EXPRESSIONS
* a regex pattern has the following structure:
        `var pattern = /(a|b|c)/gi`
    - search for matches against 'a', 'b' and 'c'
    - `g` means search globally for **all** matches, not just the first match
    - `i` makes the search case-insensitive
    ```javascript
    var str = "An apple was eaten";
    str.replace(/a/gi, "4"); // "4n 4pple w4s e4ten"

    var str = "My phone number is 524-423-4213";
    str.replace(/[0-9]/gi, "X"); // "My phone number is XXX-XXX-XXXX"
    ```



## MATHS
* be careful with prefix/postfix!
    - using postfix returns the value *before* incrementing -- prefix probably does what you want.
    - decrement works the same
    ```javascript
        var x = 3;
        var y = x++; //x = 4. y = 3
    // versus:
        var a = 6;
        var b = ++a; //a = 7. b = 7
    ```



## DOM MANIPULATION
### ACCESS EXISTING ELEMENTS
* `document` is your keyword to accessing the DOM
* get elements by id or class:
    ```javascript
        var footer_elem = document.getElementById("footer");
        ////add class
    ```
### ADDING ELEMENTS
* create an element, select something to add the element to, and then add it
    ```javascript
        // append a new item:
        var content = document.getElementById("content");
        var para = document.createElement("p"); // creates an unattached <p>
        var node = document.createTextNode("The quick fox jumped over the lazy dog.");
        para.appendChild(node);
        content.appendChild(para);
        // insert an item:
        var footer_elem = document.getElementById("footer");
        var hr_elem = document.createElement("hr");
        footer_elem.parentNode.insertBefore(hr_elem, footer_elem);
    ```
### REMOVING/REPLACING ELEMENTS
* in js you need the parent of the element to be able to remove its child
    ```javascript
        // remove:
        var child = document.getElementById("remove_me");
        child.parentNode.removeChild(child);
        // replace:
        var para = document.createElement("p");
        var node = document.createTextNode("Hi hello!");
        para.appendChild(node);
        var child = document.getElementById("replace_me");
        child.parentNode.replaceChild(para, child); // args: (new_elem, discarded_elem)
    ```