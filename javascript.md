## BACKGROUND
* ECMA2015 == ES6
* js ecosystem: javascript is most commonly run in browsers, but it can also be used in native apps
	- Electron: framework for building native apps using js, html, css etc
	- Cordova
	- NodeJS: serverside js


o javascript has myriad applications
	- building front end for responsive web pages
	- used as server-side scripting
	- building web apps
o javascript as it applies to front end dev:
	- websites are built through three aspects:
	* content (HTML)
	* style (CSS)
	* interactivity (JS)
	- each browser reads and interprets html, css and js
	- though js is often embedded in an html document, it is good to keep
	  js code in a separate .js document
	- this separate file can be reference by embedding the following code
	  in the head tag:
		<script src="myscript.js"></script>
	* this tag cannot contain other js code
	* more than one file can be linked to a single html doc
	- you can also insert js into an html doc by putting code inside
	  script tags, wherever you need to put it
		<script>
		alert("Here's an important message");
		</script>
	- js code is usually inserted just before the close of the head tag
	  and just before the close of the body tag. this allows the html to
	  be interpreted and displayed before we execute our js


## LANGUAGE FEATURES
### BASICS
* statements are semicolon-terminated
* comments
	```javascript
	// single line comment
	console.log(content);
	/* versus this
		multiple line comment */
	```


### DATA TYPES
* Numeric: always 64-bit floating point
* String
* Boolean
* infinity!
	- division by zero will also generate infinity
* Array
* Hex: `var myVar = 0xFF;`
* NaN
	- generated when trying to do arithmetic with non-numbers
* objects
* undefined



### VARIABLES
* always declare your variables, but we'll talk about undeclared variables anyway.
* variables are typically camelCase or snake-case and cannot start with a number
* variables can be declared and assigned on the same line
	```javascript
		var price, quantity, total;
		price = 10.99;
		quantity = 291;
		total = price * quantity;

		var price = 5, quantity = 291;
		var total = price * quantity;
	```
* undeclared variables do not use the `var`, `const` or `let` keyword. undeclared variables are always global
	```javascript
		function foo() {
			y = 1;      // throws an error in strict mode
			var z = 2;
		}
		foo();
		console.log(y); // 1
		console.log(z); // throws reference error
	```
* differences between variable types:
	- declared variables are created before any code is executed
	- undeclared variables do not exist until the code assigning them is executed
	- declared variables are non-configurable properties of their execution context and thus, cannot be deleted
	- undeclared variables are configurable and can be deleted
* variable hoisting:
	- this really only has to do with `var` variables because `const` variables need to be initialized when they are declared, and you'd get an error trying to reference a `let` variable before it's declared.
	- because declared variables are processed before any code is executed, declaring them anywhere in code is equivalent to declaring them at the top of the function or script.
	- hoisting only affects the variable's instantiation, not its value assignment, which will occur when the line of code where value assignment takes place is reached.
	- to be as clear as possible about the scope of variables, you should declare all variables at the top of the code block.
	```javascript
		function do_the_thing() {
			console.log(bar); // undefined (not an error)
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
		// however this doesn't work with `let`
		function uh_oh() {
			console.log(bar); // error!
			let bar = 12;
		}
	```

#### DECLARING VARIABLES WITH KEYWORDS
* `const` says "we won't be changing the value of this variable" -- use it as default if that condition applies
	- the `const` keyword declares a constant variable and it must be initialized upon declaration
	- the `const` declaration means that the identifier can't be reassigned, but its properties can be mutated
	```javascript
		const foo = bar;
		foo.name = "qux";  // legit
		foo = 12;          // illegit
		const cor;         // nope
	```
* `let` means "we may reassign this variable", as for a loop variable
	- the `let` keyword declares a block-scope variable, optionally assigning it to a value
	- `let` will only be used in the block it's defined in -- it falls out of scope once the block is cleared
	```javascript
		let x;
		x = 10;
		if(true) {
			let x = 20;
			console.log(x);     //20
		}
		console.log(x);         //10
		if(true) {
			let y = 5;
		}
		console.log(y);         // error!
	```
	- does not create a property on the global object
	```javascript
		var x = 'hello';
		let y = 'world';
		console.log(this.x); //'global'
		console.log(this.y); //undefined
	```
* `var` is the most ambiguous. in ES6, it shouldn't really be used because its meaning is so weak
	- the `var` keyword declares a variable, optionally assigning it to a value
	- default value is `undefined`
	- re-declaring a variable does not cause it to lose its value (unless you also assign it a value)
	- scope is set to "execution context"
		* if a var is declared within a function, its scope is that function
		* if a var is declared outside of a function, its scope is global

### REST PARAMETERS AND SPREAD SYNTAX
* `rest parameters` allow functions to store multiple arguments in a single array
* you can use rest parameters in tandem with normal parameters, but the rest param must come last.
	- this is how "rest parameters" got their name -- they're the "rest" of the parameters
	- as such, you can only have one rest param per function
	```javascript
		function sendCars(day, ...allCarids) {
			console.log('Today is ' + day);
			allCarids.forEach( id => console.log(id) );
		}
		sendCars('Monday', 100, 200, 555);
	```
* `spread syntax` looks a lot like `rest parameter` syntax, but it works in the opposite direction. spread syntax works great with `iterables`
	```javascript
		function startCars(car1, car2, car3) {
			console.log(car1, car2, car3);
		}
		let carIds = [100, 300, 500];
		startCars(...carIds);       // 100 300 500
		// since strings are "iterables" it works for strings as well:
		let carCodes = 'abc';
		startCars(...carCodes);     // breaks up the string 'abc' and logs each char separately as 'a' 'b' 'c'
		// using rest and spread at the same time:
		function startCars(car1, car2, car3, ...rest) {
			console.log(rest);
		}
		let carIds = [1, 2, 3, 4, 5, 6];
		startCars(...carIds);       // [4, 5, 6]
	```

### DESTRUCTURING
* allow us to easily assign variables to an array
* we can use rest parameter syntax when we destructure
* destructuring arrays:
	```javascript
		let carIds = [1, 2, 5];
		let [car1, car2, car3] = carIds;    // this is the destructuring of the array
		console.log(car1, car2, car3);      // 1 2 5
		// with rest parameter syntax
		let fruits = ["apple", "pear", "orange", "kiwi"];
		let snack, remSnacks;
		[snack, ...remSnacks] = fruits;
		console.log('I ate my ' + snack + ' and I have '+ remSnacks + ' left.');
		// skip the first elem
		let carIds = [1, 2, 5];
		let remainingCars;
		let [, ...remainingCars] = carIds;  // skips the first elem and assigns the rest to remainingCars
		console.log(remainingCars);         // [2, 5]
	```
* destructuring objects has a different syntax than destructuring arrays:
	```javascript
		let car = { id: 500, style: 'convertible' };
		let { id, style } = car;
		console.log(id, style);             // 500, 'convertible'
	```

### DESTRUCTURING OBJECTS


### SPREAD SYNTAX




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
* properties of an object can be accessed using dot or bracket notation
	- dot notation is generally easier to read, but using bracket notation allows us to use variable to access properties
		```javascript
			let farm = {
				cat: 'meow',
				dog: 'woof',
				cow: 'moo',
				chicken: 'cluck'
			};

			let sound = farm.dog; // 'woof'
			let animal = 'cow';
			sound = farm[animal]; // 'moo'
		```
	- `for` can be used to iterate through the properties of an object
		```javascript
			for ( var propName in student) {
				console.log(propName); // lists the properties
				console.log(student[propName]); // lists the value of prop
			}
		```
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
	- if you have in-line script (`document.write('Good afternoon!')`, say) and put it halfway through your document, 'Good afternoon!' will be inserted at that halfway point in the document.

### NODES
* below the parent `document` object, the DOM consists of a tree of objects called nodes
* there are three types of nodes:
	- elements (`<h3>`)
	- text ("Hello, welcome to Earth!")
	- attribute (css style)

### ACCESS EXISTING ELEMENTS
* `document` is your keyword to accessing the DOM
* get elements by id or class:
	```javascript
		// returns a single element
		var footer_elem = document.getElementById("footer");

		// returns a collection, so do some processing:
		var elems = document.getElementsByClassName("foo");
		var i;
		for(i = 0; i < elems.length; i++) {
			elems[i].style.backgroundColor = "red";
		}
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





# OTHER STUFF


### COMPARISONS AND FALSY VALUES
* "Falsy" and "truthy" values: value/type combos that evaluate to true or false via type coercion in JS's internal `ToBoolean` function which underlies statements like `!value`, `value ? ... : ... ;` and `if(value)`
* falsy values include:
	- `false` - the true boolean 'false'
	- `0`, `0.0`, `-0`, `0x0` - numerical zeroes
	- `""`, `''` - empty strings
	- `null` - fun fact: `typeof null` returns `'object'`
	- `undefined`
	- `NaN` - not a number
* Falsy values do not all necessarily compare to each other via loose comparison `==`
	- `false`, numerical zeroes and empty strings are equal under loose comparison
	- `null` will not compare to any other value but `null` and `undefined`
	- `NaN` does not compare to any other value, *not even itself*
	```javascript
		false == 0;         // true
		0 == "";            // true
		"" == false;        // true
		null == undefined;  // true
		NaN == NaN;         // false...... wtf
	```
* "Truthy" values that are actually false via loose comparison: there are some values that will evaluate true in an `if(value)` statement but `value != true`
	- `"0"`, `'0'` - non-empty strings are truthy, but evaluate to false when using the comparison operator
	- `new Number(0)` and `new Boolean(false)` are objects which are truthy, but the comparison operator sees their values, which are false
	- `0 .toExponential();` an object with a numerical value equivalent to `0`
	- `[]`, `[[]]` and `[0]`
* other Truthy values you might expect to be falsey:
	- `-1` and other negative non-zero negative numbers
	- `" "`, `' '`, `"false"`, `'null'` and all other non-empty strings
	- anything from `typeof` which always returns a non-empty string
	- any object (aside from fake object `null`) including:
		* `{}`, `[]`, `function(){}` or `() => {}`
	- any regex
	- any symbol
* for objects, the rules relate to reference i think...
	- for two identical objects, they will fail comparison of either type.
	- if two objects share the same reference (they point to the same object) then they are equivalent via both loose and strict comparison



## CONDITIONS AND LOOPS
#### CONDITIONS
* there is a ternary shorthand for an `if-then-else` statement. It can be used in places where a regular if-else block cannot (inside a condition or method parameter)
	```javascript
	// standard 'if' block:
	if (name === "Dave") {
		alert("Hey, I know you!");
	} else
		alert("I don't know you, man");
	}
	// or use ternary:
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
* **loose equality** `==` performs **type coercion** first and then attempts comparison
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
* `forEach` is very popular as it typically is more readable than `for` and decently performant for most applications
	```javascript
		const map = [
			{name: "Sally", age: 32},
			{name: "Edgar", age: 12},
			{name: "Josephine", age: 39},
			{name: "Mark", age: 20},
		];

		map.forEach( item => (console.log(item.name + " is " + item.age + " years old.")));
	```
* `while`
	```javascript
		let counter = 0;
		while (counter < 10) {
			counter++;
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


## STRINGS AND STRING METHODS
o can be demarcated with '' or ""
o you can use an escape character to express ' within a '' block or to
  express " within a "" block.
	  var quotation = "\"Elementary, my dear Watson.\" --Sherlock Holmes";
o whitespace
o concatenation: + symbol
	var name = prompt("What is your name?");
	alert("Hello, " + name);
o you can use += to concatenate strings
	message += " Thanks for stopping by!";
	- this technique can be used to build up large blocks of text in a
	  more readable way than using a million concatenation operators

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
		* `*` matches zero or more occurrences
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
	- **line boundaries**:
		* `^` denotes beginning of a line
		* `$` denotes end of a line
		```javascript
			^(the cat).+    // 'the cat' must be at the start of the line.
							// matches 'the cat runs' but not 'see the cat run'
			.+(the cat)$    // 'the cat' must be at the end of the line.
							// matches 'watch the cat' but not 'the cat eats'
			^.$             // matches all strings containing just one character
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


JAVASCRIPT CONSOLE:
o chrome has a js console built in. just type command+alt+j to open it
o this displays errors
o sometimes it's buggy and doesn't display the line number. refreshing
  should fix it.
o use 'console.log' to print messages in the console when you execute your
  page in the browser
	in myscript.js:
		console.log("program has finished running");
	- this is really awesome for tracking variable content, debugging, etc
	- try wrapping your .js code with console log calls:
		console.log("begin program");
		.... some code ....
		console.log("end program");
o use clear() to clear it out
o press up to scroll through the history
