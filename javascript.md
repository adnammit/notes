

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
    - the two args sent to `substr()` are the index where the substr starts and how many chars long it is
    ```javascript
    var str = "Hello planet Earth";

    var part1 = str.substr(6,6); // "planet"
    str.substr(6,6).toUpperCase(); // "Hello PLANET Earth"
    ```    

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
