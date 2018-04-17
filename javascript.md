
### OBJECTS AND PROPERTIES
* objects may have:
    - properties
    - methods
    - events
* the browser has a special relationship with the `window` and `document` objects -- their properties and methods are already written and ready to be used
    - document
        * props: URL, title, lastModified
        * events: load, click, keypress
        * methods: write(), getElementById()
    - window
        * properties: location (url)

* an **object literal** is a comma-separated list of name-value pairs wrapped in curly braces
    - may be on one line or multiple
    - property values can be of any type including arrays and functions
    - there is no comma after the last item
    - curly braces are terminated with a semi colon
    ```javascript
        var Swapper = {
            images: ["smile.gif", "grim.gif", "frown.gif", "bomb.gif"],
            pos: {
                x: 40,
                y: 300
            },
            onSwap: function() {
                // do stuff
            }
        };
    ```
    - why use an object literal?
        * useful for unobtrusive event handling
    - why not use them?
        * it's a bit easier to introduce syntax errors -- complex ones can be difficult to navigate

* objects can be declared using brace notation:
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
* use the `hasOwnProperty` method to check that the prop belongs to the object and is not inherited


## THE DOCUMENT OBJECT MODEL
### LOADING A WEB PAGE
* when a webpage is loaded, the process looks like this:
    - the page is received as html
    - the document object and all its nodes are stored in memory
    - the rendering engine processes any supplied CSS rules
* when is JS loaded?
    - as the browser loads the document, when it finds a `script` tag it stops and loads it, and does whatever it needs to do
    - if you have in-line script and put it halfway through your document, `document.write('Good afternoon!')` will be inserted at that halfway point in the document.

### NODES
* below the parent `document` object, the DOM consists of a tree of objects called nodes
* there are three types of nodes:
    - elements (`<h3>`)
    - text ("Hello, welcome to Earth!")
    - attribute (css style)

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


## SYNTAX
### BASICS    
* statements are semicolon-terminated
* comments
    `// single line comment
    /* versus this
        multiple line comment */ `

#### COMPARISONS AND FALSY VALUES        
* "Falsy" values: value/type combos that evaluate to false via type coercion:
    - `false` - the true boolean 'false'
    - `0` - numerical zero
    - `""` - empty string
    - `null`
    - `undefined`
    - `NaN` - not a number
* Falsy values do not all necessarily compare to each other via loose comparison `==`
    - `false`, `0` and `""` are equal under loose comparison
    - `null` will not compare to any other value but `null` and `undefined`
    - `NaN` does not compare to any other value, *not even itself*
    ```javascript
        false == 0;         // true
        0 == "";            // true
        "" == false;        // true
        null == undefined;  // true
        NaN == NaN;         // false...... wtf
    ```
* for objects, the rules relate to reference i think...
    - for two identical objects, they will fail comparison of either type.
    - if two objects share the same reference (they point to the same object) then they are equivalent via both loose and strict comparison

### DATA TYPES
* Numeric
* String
* Boolean

### VARIABLES
* always declare your variables, but we'll talk about undeclared variables anyway.
* variables can be declared and assigned on the same line
    ```javascript
        var price, quantity, total;
        price = 10.99;
        quantity = 291;
        total = price * quantity;

        var price = 5, quantity = 291;
        var total = price * quantity;
    ```
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

## CONDITIONS AND LOOPS
#### CONDITIONS
* there is a ternary shorthand for an `if-then-else` statement. It can be used in places where a regular if-else block cannot (inside a condition or method parameter)
    ```javascript
    // return a priced depending on whether someone is a member or not
    function getFee(isMember) {
        return (isMember ? "$2.00" : "$4.00");
    }

    // make the string uppercase or lowercase depending on the flag
    var str = caseFlag ? str.toUpperCase() : str.toLowerCase();
    ```
#### COMPARISONS
* js has two ways of testing **equality**: `==` and `===`
* **strict equality** `===` means both type and value must be the same
    ```javascript
        'hello' === 'hello';    // true
        5 === 5;                // true
        7 === '7';              // false
        false === 0;            // false
    ```
* **loose equality** `==` performs **type coercion** first and then attempts comparision
    ```javascript
        7 == '7';               // true -- '7' converts to integer 7
        false == 0;             // true -- 0 converts to false, they are both 'falsy' values
    ```
#### FOR LOOPS
* using a `for` loop to iterate through items in an array:
    ```javascript
        var str = "Hello I am a very fine piece of hardware";
        var words = str.split(" ");
        for (var i = 0; i < words.length; i++) {
            console.log(words[i]);
        }
    ```

* use `for-in` to enumerate through object properties. This is not appropriate for arrays.
    ```javascript
        var obj = {
            firstName: "Wesley",
            lastName: "Crusher",
            age: "16"
        };
        for (var prop in obj) {
            if(obj.hasOwnProperty(prop)) {  // hasOwnProperty means the props are not inherited
                alert("Property "+prop+" has value: "+obj[prop]);
            }
        }
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
* str length is just a prop of a string object:
    ` var len = str.length;`

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
* you can do simple tests of regexes in the console with:
    `console.log(/cde/.test("abcdef"))`
* a regex pattern has the following structure:
        `var pattern = /(a|b|c)/gi    //search for matches against 'a', 'b' and 'c'`
    - `g` means search globally for *all* matches, not just the first match
    - `i` makes the search case-insensitive
    - `[]` brackets denote a set of potential matches such as all digits `/[0-9]/`, all letters `/[a-z]/i`
        * note that special chars lose their special meaning inside brackets
    ```javascript
    var str = "An apple was eaten";
    str.replace(/a/gi, "4"); // "4n 4pple w4s e4ten"

    var str = "My phone number is 524-423-4213";
    str.replace(/[0-9]/gi, "X"); // "My phone number is XXX-XXX-XXXX"

    // Capitalize the first letter of each word using word boundary [\b]
    var str = "hello my name is sally";
    var capitalized = str.replace(/\b[a-z]/gi, function(char) {
        return char.toUpperCase(); // "Hello My Name Is Sally"
    });
    ```
* special characters:
    - `\d` any digit
    - `\w` an alphanumeric character
    - `\s` any whitespace character (space, tab, newline)
    - `\D` any char that is *not* a digit
    - `\W` a non-alphanumeric character
    - `\S` a non-whitespace character
    - `.` any char except newline
        `.art` matches `dart` and `cart` but not `art` or `start`
    - `^` the inversion of; matches any char but what is in the set
        ```javascript
        /[^01].test("100100100010");  // false
        /[^01].test("100102100010");  // true
        ```
    - **repeating expressions** can be used to match zero or more expressions.
        * `+` matches one or more occurrences of the regex
        * `*` matches zero or more occurences
        * `?` matches zero or one time
        ```javascript
        /boo+(hoo+)+/i.test("Boohooohoohooo"); //true
        /boo+(hoo+)+/i.test("Boohoofffff"); //true
        /boo+(hoo+)+/i.test("Boohohoohoo"); //false - must be 'boo' (plus o's), 'hoo' (plus o's) opt repeated 'hoo's
        /neighbou?r/.test("neighbor"); //true
        /neighbou?r/.test("neighbour"); //true
        ```
    ```javascript
        a+b     // match one or more occurrences of 'a' followed by one occurrence of 'b'
                // matches ab, aaab, abb         does not match b, baa

    ```
    - **word boundaries**:
        * `\b` denotes the beginning of a word
    ```javascript
        var str = "the very voracious vulture vaulted upwards into the sky";
        var vstrs = str.match(/\bv+/gi);
    ```
    <!-- - **line boundaries**:
        * `^` denotes beginning of a line
        * `$` denotes end of a line
    ```javascript
        ^(the cat).+    // 'the cat' must be at the start of the line.
                        // matches 'the cat runs' but not 'see the cat run'
        .+(the cat)$    // 'the cat' must be at the end of the line.
                        // matches 'watch the cat' but not 'the cat eats'
        ^.$             // matches all strings containing just one character
    ``` -->

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
