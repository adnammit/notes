# Javascript

## References & Resources
* [Eloquent Javascript](https://eloquentjavascript.net/)
* [The Complete JS Course](https://broadlume.udemy.com/course/the-complete-javascript-course)
* [JSON Placeholder](https://jsonplaceholder.typicode.com/): easy to use fake data API for testing and prototyping

## Important JS Concepts
* destructuring object and arrays
* spread/rest syntax
* template literals
* ternaries
* arrow functions
* short-circuit and logical operators (`&&`, `||`, `??`)
* optional chaining (`.?`)
* mutable array operations vs immutable array operations
* promises
* async/await

## Stuff To Understand Better
* chunking
* modules
* module federation
* bundling
* rollups
* SSR


## Cool Tricks/Gotchas/Best Practices
* developer tools:
	* use Quokka.js vscode extension to run js code in the editor
	* use [**live-server**](https://github.com/ritwickdey/vscode-live-server) vscode extension or command line to easily run your html/js with hot reloading -- nice for actually viewing a website
* use `const` or `let` if possible -- fallback to `var` if necessary
* [`truthy values`](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) are anything that is not falsy
* [`falsy values`](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) are `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, `NaN`, and `document.all`
* use `!!` to cast a non-boolean to a boolean -- particularly if you need to assign a boolean based on the truthiness of another value:
	```js
		let enabled = !!userId;
		let enabled = Boolean(userId); // same as this
		// or consider the difference between these assignments:
		let isValid = !!(list && list.name && list.name.trim()); // assigns a boolean
		let name = list && list.name && list.name.trim(); // assigns a string
	```
* use **short circuit evaluation** for variable assignment when you want to assign to the first non-null/empty value
	* the last value will be assigned even if it's empty/null so it's important to put a default value at the end if you need some value
	```js
		var a;
		var b = "";
		var c = null;
		var d = a || b || c || "hello"; // d == "hello"
	```
	* you can use either nullish coalescing (`??`) or logical OR (`||`) for this -- `||` is looser as it checks for falsy values so `0` and `""` will evaluate to false
	```js
		let name = "" || "default name"; // "default name"
		let name = "" ?? "default name"; // ""
	```
* **copying data**: TL;DR: use `structuredClone` for deep copies
	* [comparison of all options](https://web.dev/articles/structured-clone)
	* for **shallow copies** use `Object.assign({}, obj)` to create a copy of `obj` -- it copies values but not references, so the copy will be linked to any nested objects in the original
	* you can also use spread syntax for a **shallow copy** 
	* for **deep copies** of objects and arrays you can use `JSON` or `structuredClone`
		* `JSON` is more widely supported, but cannot handle cyclical data structures or built in types like `Date`, `Map`, `Set` etc
		* `structuredClone` is faster, handles cyclical data structures, and can handle built in types
		* `lodash cloneDeep()` provides the most flexibility in deep cloning but requires the library
	```js
		// shallow copies
		var copy = Object.assign({}, original);
		var copy = {...original};
		// deep copies
		var copy = JSON.parse(JSON.stringify(original));
		var copy = structuredClone(original);
	```
* to **merge object or arrays** check out `lodash mergeWith()` ([ref](https://www.geeksforgeeks.org/lodash-_-mergewith-method/))
	* `mergeWith()` with a customizer function can be used to merge objects or arrays with custom logic
	```js
		const _ = require("lodash"); 
		
		function customizer(obj, src) { 
			if (_.isArray(obj)) { 
				return obj.concat(src); 
			} 
		} 
		
		let object = { 
			'amit': [{ 'susanta': 20 }, { 'durgam': 40 }] 
		}; 
		
		let other = { 
			'amit': [{ 'chinmoy': 30 }, { 'kripamoy': 50 }] 
		}; 
		
		console.log(_.mergeWith(object, other, customizer));
		// output:
		// { 'amit': [{'susanta': 20 }, { 'durgam': 40}, {'chinmoy': 30}, {'kripamoy': 50 } ]} 
	```
	* the customizer is important to be able to combine arrays which are not seen as the same objects by default -- merging the below objects using `merge()` or `mergeWith()` without the customizer function would result in 
	```js
		{ 'amit': [{'chinmoy': 30, 'susanta': 20 }, { 'durgam': 40, 'kripamoy': 50 }] }
	```
* **sort an array**: `sort` mutates in place so you can use spread syntax or `splice` to create a copy, or if it is supported, use `toSorted` (2023)
* attaching js to html: import script in body (after DOM is loaded) unless there's a good reason to put it in document head
* is js asynchronous? not really, but it behaves like it thanks to web/browser API and promises which handle events concurrent to the main call stack and emit callback events that are eventually pushed back on to the main call stack. priority of execution is as follows:
	1. synchronous events (`console.log("hello world")`)
	2. promise resolution (`new Promise(resolve()).then(() => console.log("resolved!"))`)
	3. browser API events (`setTimeout(() => (console.log("time's up!")), 0)`) << even with 0 timeout, this executes last
* [debugging beyond `console.log()`](https://medium.com/@anirudh.munipalli/stop-using-console-log-in-javascript-try-these-instead-72490d895a24)
	* use `console.trace()` to see the call stack of the function that calls it
	* use `console.group()` and `console.groupEnd()` to group related messages together
	* use `console.time()` and `console.timeEnd()` to see how long a process takes to execute
	* use `console.table()` to organize complex data: `console.table({person1, person2, person3})`
	* add css to the console to make your logs stand out: `console.log("%cHello World", "color: blue; font-size: 20px; border: solid;")`
* **array manipulation**
	* use `shift` over `splice` to remove the first element of an array

## Background
* js ecosystem: javascript is most commonly run in browsers, but it can also be used in native apps
	* Electron: framework for building native apps using js, html, css etc
	* Cordova
	* NodeJS: serverside js
* javascript has myriad applications
	* building front end for responsive web pages
	* used as server-side scripting
	* building web apps
* javascript as it applies to front end dev:
	* websites are built through three aspects:
		* content (HTML)
		* style (CSS)
		* interactivity (JS)
	* each browser reads and interprets html, css and js
	* though js is often embedded in an html document, it is good to keep js code in a separate .js document
	* this separate file can be reference by embedding the following code in the head tag:
		<script src="myscript.js"></script>
		* this tag cannot contain other js code
		* more than one file can be linked to a single html doc
			* you can also insert js into an html doc by putting code inside script tags, wherever you need to put it
				<script>
				alert("Here's an important message");
				</script>
			* js code is usually inserted just before the close of the head tag and just before the close of the body tag. this allows the html to be interpreted and displayed before we execute our js
* js flavors:
	* **ES5** (ECMA2015) is the standard version of js that is supported by all browsers
	* **ES6** is the latest version of js and is supported by most modern browsers
	* **CommonJS** is a project to standardize server-side js -- it is used by node.js. it has a unique import/require syntax for modules that cannot be used in the browser

# Language Features

## Basics
* statements are semicolon-terminated
* comments:
	```javascript
	// single line comment
	console.log(content);
	/* versus this
		multiple line comment */
	```

## Runtime Environment
* [more info](https://medium.com/@monuchaudhary/single-threaded-non-blocking-asynchronous-and-concurrent-nature-of-javascript-a0d5483bcf4c)
* the js runtime is what is run by the browser (or server in the case of node/server-side js). for chrome, the engine is V8
* runtime engine consists of:
	* call stack and a memory heap
	* event queue/callback queue for browser APIs
	* job queue for promises
	* event listeners, http/ajax requests, and timing functions
	* event loop that constantly checks for completed events from the web API/promises and moves tasks from the queue to the call stack
* js is **single-threaded** -- it has a single call stack and handles instructions one at a time
* js **non-blocking** -- when it encounters an API call, a promise, an AJAX request, an I/O event or timer, it does not wait -- it moves on to the next code on the stack
* js is able to be non-blocking with only a single thread because requests are performed by web APIs (C++ library for node) which has its own thread. when the request completes, the callback function is sent to the **callback queue**/**event queue**, or in the case of a promise, to the **job queue** (more on that below)
* though js is executed synchronously, it behaves **asynchronously** and **concurrently** thanks to the web API/promises handling events concurrently in the background, allowing commands in the the call stack to continue to execute

### Functions and the Call Stack
* the runtime has one **call stack**
* the **execution context** is the function at the top of the call stack currently being executed
* the call stack executes functions sequentially, line-by-line, one by one
* call stack is **FIFO**: when a function calls another function, the new function is pushed on top of the stack and executed, then popped off to continue executing the calling function

### Browser API and the Callback Queue
* so we have a thread executing the call stack and a thread handling api calls
* when we make an async function call in code and that code is executed, the function is pushed to the call stack
* the call is made to the web API and the function is popped off the call stack
* additional calls are executed on the call stack while the api processes the request
* the api pushes callback functions onto the callback/event queue as they're complete -- this is called a **macro task**
* macro tasks are executed when the call stack is empty -- the event loop is constantly checking whether the call stack is empty and if so, if there are callback events to move to the call stack

### Promises and the Job Queue
* [more info](https://www.freecodecamp.org/news/synchronous-vs-asynchronous-in-javascript/#:~:text=JavaScript%20is%20a%20single%2Dthreaded,language%20with%20lots%20of%20flexibility.)
* **promises** are also executed asynchronously like ajax calls and browser events, but they are handled in a separate queue from web API tasks
* promises need an **executor function** which specifies what happens when the promise resolves or rejects (`then()` and `catch()` methods respectively)
* promise executor functions are handled by the **job queue** rather than the **callback queue**
* the executor function is put onto the **job queue** similar to how completed requests to the browser API are put on the callback queue
* the job queue emits **micro tasks** whereas the callback queue emits **macro tasks**
* micro tasks take precedence over macro tasks: as the event loop runs, it will handle *all* micro tasks first before moving to check the callback queue

### Execution Example
* put all together, we can see the combined execution of browser API events and promises:
	```js
	function f1() {
		console.log('f1');
	}

	function f2() {
		console.log('f2');
	}

	function main() {
		console.log('main');

		setTimeout(f1, 0);

		new Promise((resolve, reject) =>
			resolve('I am a promise')
		).then(resolve => console.log(resolve))

		f2();
	}

	main();
	```
* this will execute as follows. notice that it does not matter if setTimeout has delay of 0, it still gets handled last
	```sh
	main
	f2
	I am a promise
	f1
	```


## Data Type Overview
* Numeric: always 64-bit floating point
* String
* Boolean
* infinity!
	* division by zero will also generate infinity
* Array
* Hex: `var myVar = 0xFF;`
* NaN
	* generated when trying to do arithmetic with non-numbers
* objects
* undefined

## Comparisons And Falsy Values
* "Falsy" and "truthy" values: value/type combos that evaluate to true or false via type coercion in JS's internal `ToBoolean` function which underlies statements like `!value`, `value ? ... : ... ;` and `if(value)`
* falsy values include:
	* `false` - the true boolean 'false'
	* `0`, `0.0`, `-0`, `0x0` - numerical zeroes
	* `""`, `''` - empty strings
	* `null` - fun fact: `typeof null` returns `'object'`
	* `undefined`
	* `NaN` - not a number
* Falsy values do not all necessarily compare to each other via loose comparison `==`
	* `false`, numerical zeroes and empty strings are equal under loose comparison
	* `null` will not compare to any other value but `null` and `undefined`
	* `NaN` does not compare to any other value, *not even itself*
	```javascript
		false == 0;         // true
		0 == "";            // true
		"" == false;        // true
		null == undefined;  // true
		NaN == NaN;         // false...... wtf
	```
* "Truthy" values that are actually false via loose comparison: there are some values that will evaluate true in an `if(value)` statement but `value != true`
	* `"0"`, `'0'` - non-empty strings are truthy, but evaluate to false when using the comparison operator
	* `new Number(0)` and `new Boolean(false)` are objects which are truthy, but the comparison operator sees their values, which are false
	* `0 .toExponential();` an object with a numerical value equivalent to `0`
	* `[]`, `[[]]` and `[0]`
* other Truthy values you might expect to be falsy:
	* `-1` and other negative non-zero negative numbers
	* `" "`, `' '`, `"false"`, `'null'` and all other non-empty strings
	* anything from `typeof` which always returns a non-empty string
	* any object (aside from fake object `null`) including:
		* `{}`, `[]`, `function(){}` or `() => {}`
	* any regex
	* any symbol
* for objects, the rules relate to reference i think...
	* for two identical objects, they will fail comparison of either type.
	* if two objects share the same reference (they point to the same object) then they are equivalent via both loose and strict comparison

## Rest Parameters And Spread Syntax
* `rest parameters` allow functions to store multiple arguments in a single array
* you can use rest parameters in tandem with normal parameters, but the rest param must come last.
	* this is how "rest parameters" got their name -- they're the "rest" of the parameters
	* as such, you can only have one rest param per function
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

## Destructuring
* allow us to easily assign variables to an array
* we can use rest parameter syntax when we destructure
* destructuring arrays:
	```javascript
		let carIds = [1, 2, 5];
		let [car1, car2, car3] = carIds;    // destructure the array
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
		let { id, style } = car;			// destructure the object
		console.log(id, style);             // 500, 'convertible'
	```
* this seems dangerous. what happens if you destructure an array or object that is empty?
	* you can check if the array/object is empty/null before destructuring
	* you can specify a default 
	```js
		let carIds = [];
		let [car1 = 1, car2 = 2, car3 = 3] = carIds;
		console.log(car1, car2, car3);      // 1 2 3
	```


## Regular Expressions
* you can do simple tests of regexes in the console with:
	`console.log(/cde/.test("abcdef"))`
* a regex pattern has the following structure:
		`var pattern = /(a|b|c)/gi    //search for matches against 'a', 'b' and 'c'`
	* `g` means search globally for *all* matches, not just the first match
	* `i` makes the search case-insensitive
	* `[]` brackets denote a set of potential matches such as all digits `/[0-9]/`, all letters `/[a-z]/i`
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
	* `\d` any digit
	* `\w` an alphanumeric character
	* `\s` any whitespace character (space, tab, newline)
	* `\D` any char that is *not* a digit
	* `\W` a non-alphanumeric character
	* `\S` a non-whitespace character
	* `.` any char except newline
		`.art` matches `dart` and `cart` but not `art` or `start`
	* `^` the inversion of; matches any char but what is in the set
		```javascript
		/[^01].test("100100100010");  // false
		/[^01].test("100102100010");  // true
		```
	* **repeating expressions** can be used to match zero or more expressions.
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
	* **word boundaries**:
		* `\b` denotes the beginning of a word
		```javascript
			var str = "the very voracious vulture vaulted upwards into the sky";
			var vstrs = str.match(/\bv+/gi);
		```
	* **line boundaries**:
		* `^` denotes beginning of a line
		* `$` denotes end of a line
		```javascript
			^(the cat).+    // 'the cat' must be at the start of the line.
							// matches 'the cat runs' but not 'see the cat run'
			.+(the cat)$    // 'the cat' must be at the end of the line.
							// matches 'watch the cat' but not 'the cat eats'
			^.$             // matches all strings containing just one character
		```

## Maths
* be careful with prefix/postfix!
	* using postfix returns the value *before* incrementing -- prefix probably does what you want.
	* decrement works the same
	```javascript
		var x = 3;
		var y = x++; //x = 4. y = 3
	// versus:
		var a = 6;
		var b = ++a; //a = 7. b = 7
	```


# Variables
* variables should be declared using `const` or `let` keywords whenever possible. use `var` as a last resort
* always declare your variables, but we'll talk about undeclared variables anyway
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

## Variables Keywords
* in ES6 we got all the variable types
* `const` says "we won't be changing the value of this variable" -- use it as default if that condition applies
	* the `const` keyword declares a constant variable and it must be initialized upon declaration
	* the `const` declaration means that the identifier can't be reassigned, but its properties can be mutated
	```javascript
		const foo = bar;
		foo.name = "qux";  // legit
		foo = 12;          // illegit
		const cor;         // nope
	```
* `let` means "we may reassign this variable", as for a loop variable
	* the `let` keyword declares a block-scope variable, optionally assigning it to a value
	* `let` will only be used in the block it's defined in -- it falls out of scope once the block is cleared
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
	* does not create a property on the global object
	```javascript
		var x = 'hello';
		let y = 'world';
		console.log(this.x); //'global'
		console.log(this.y); //undefined
	```
* `var` is the most ambiguous. in ES6, it shouldn't really be used because its meaning is so weak
	* the `var` keyword declares a variable, optionally assigning it to a value
	* default value is `undefined`
	* re-declaring a variable does not cause it to lose its value (unless you also assign it a value)
	* scope is set to "execution context"
		* if a var is declared within a function, its scope is that function
		* if a var is declared outside of a function, its scope is global


## Undeclared Variables
* undeclared variables do not use any keyword for declaration. undeclared variables are always global
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
	* declared variables are created before any code is executed
	* undeclared variables do not exist until the code assigning them is executed
	* declared variables are non-configurable properties of their execution context and thus, cannot be deleted
	* undeclared variables are configurable and can be deleted

## Variable Hoisting
* this really only has to do with `var` variables because `const` variables need to be initialized when they are declared, and you'd get an error trying to reference a `let` variable before it's declared.
* because declared variables are processed before any code is executed, declaring them anywhere in code is equivalent to declaring them at the top of the function or script.
* hoisting only affects the variable's instantiation, not its value assignment, which will occur when the line of code where value assignment takes place is reached.
* to be as clear as possible about the scope of variables, you should declare all variables at the top of the code block.
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


# Objects And Properties
* objects may have:
	* properties
	* methods
	* events
* the browser has a special relationship with the `window` and `document` objects -- their properties and methods are already written and ready to be used
	* document
		* props: URL, title, lastModified
		* events: load, click, keypress
		* methods: write(), getElementById()
	* window
		* properties: location (url)
* properties of an object can be accessed using dot or bracket notation
	* dot notation is generally easier to read, but using bracket notation allows us to use variable to access properties
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
	* `for` can be used to iterate through the properties of an object
		```javascript
			for ( var propName in student) {
				console.log(propName); // lists the properties
				console.log(student[propName]); // lists the value of prop
			}
		```
* an **object literal** is a comma-separated list of name-value pairs wrapped in curly braces
	* may be on one line or multiple
	* property values can be of any type including arrays and functions
	* there is no comma after the last item
	* curly braces are terminated with a semi colon
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
	* why use an object literal?
		* useful for unobtrusive event handling
	* why not use them?
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


# Arrays
* fun fact: arrays are objects (typeof returns `object`)
* some array methods create copies of the array, while others mutate the original array
	* `map`, `filter`, and `reduce` return a new array without altering the original
	* `sort`, `reverse`, `splice`, `pop`, `push`, `shift`, `unshift` alter the original array
* `map` is used to create a new array that transforms each element in the source array
	```javascript
		var values = [1, 2, 3, 4, 5];
		var double = values.map(x => x * 2);
		console.log(double); // [2, 4, 6, 8, 10]
	```
* `filter` is used to create a new array that contains only elements that satisfy a given condition
	```javascript
		var values = [1, 2, 3, 4, 5];
		var evens = values.filter(x => x % 2 === 0);
		console.log(evens); // [2, 4]
	```
* `reduce` is used to create a single new value by combining all elements in the source array
	```javascript
		var values = [1, 2, 3, 4, 5];
		var sum = values.reduce((acc, val) => acc + val, 0);
		console.log(sum); // 15
	```
* `sort` will sort the array in place and return the sorted array
	* sort works by default on strings
	```javascript
		var names = [ 'Susannah', 'Fred', 'Toby', 'Gina', 'Anne'];
		names.sort(); // [ 'Anne', 'Fred', 'Gina', 'Susannah', 'Toby' ]
	```
	* if you want to sort an array of numbers, you'll need to provide a comparison function
	```javascript
		var values = [3, 1, 2, 5, 4];
		values.sort((a, b) => a - b); // ascending - reverse a and b to sort in descending order
		console.log(values); // [1, 2, 3, 4, 5]
		// create a new sorted array:
		var sorted = [...values].sort((a, b) => a - b);
		// or using slice:
		var sorted = values.slice().sort((a, b) => a - b);
		// as of 2023 you can now use toSorted:
		var sorted = values.toSorted((a, b) => a - b);
	```


# Functions
* you can use `arrow functions` or `function expressions` to declare a function
	```js
		// the standard function declaration
		function add(a, b) {
			return a + b;
		}

		// as an arrow function (function expression)
		const add = (a, b) => a + b;

		// as an arrow function with a block -- note `return` is required
		const add = (a, b) => {
			return a + b;
		}
	```

# Strings And String Methods
* can be demarcated with '' or ""
* you can use an escape character to express ' within a '' block or to express " within a "" block.
	```js
		var quotation = "\"Elementary, my dear Watson.\" --Sherlock Holmes";
	```
* whitespace
* concatenation: + symbol
	var name = prompt("What is your name?");
	alert("Hello, " + name);
* you can use += to concatenate strings
	```js
		message += " Thanks for stopping by!";
	```
* this technique can be used to build up large blocks of text in a more readable way than using a million concatenation operators
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
	* `replace()` only works on the first instance -- for more mileage, use regex
	```javascript
	var str = "Hello planet Earth";

	str.replace("Earth","Mars"); // "Hello planet Mars"
	str.replace("e","a"); // Hallo planet Earth -- only replaces the first instance!
	str.split("e").join("a"); // "Hallo planat Earth" -- hacky but it works!

	var str2 = "Duck Duck Goose";
	str2.split("Duck").join("Pheasant"); // "Pheasant Pheasant Goose"
	```

* substring
	* the two args sent to `substr()` are the index where the substring starts and the length of the substring
	```javascript
	var str = "Hello planet Earth";

	var part1 = str.substr(6,6); // "planet"
	str.substr(6,6).toUpperCase(); // "Hello PLANET Earth"
	```
	* you can also use the `substring()` method which takes two args for the start of the substring and the ending index of the substring



# Pass By Reference Vs Pass By Value
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
	* when this happens, the original pointer points to the old value and a new pointer points to the new data location, so when you return out of your function, the original pointer is still pointing to the old value
	* if you want to keep your reference to the original object, you must manipulate individual elements of that object rather than replacing it entirely
	* for arrays, using methods which manipulate the array in-place (`push`, `push`, `splice`) will preserve your original reference, while methods like `concat`, `slice`, `map` and `filter` will return a new array
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



# Operators And Expressions

## Equality
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

## Logical Operators
* `!` is the logical NOT operator and can be doubled up to convert a non-boolean value to a boolean
* `&&` or `logical AND` will evaluate both operands and return true if both operands are true
* `||` or `logical OR` will evaluate the right operand only if the first is falsy and return true if either is true
* `??` or `nullish coalescing operator` evaluates the right hand side operand when the left hand side operand is `null` or `undefined`
	```javascript
		let x = null;
		let y = x ?? 5; // 5
	```
* `?.` or `optional chaining` operator only evaluates the right hand side operand if the left hand side operand is not `null` or `undefined` -- avoid null reference errors
	```javascript
		let x = null;
		let y = x?.name; // undefined
	```javascript
		let x = null;
		let y = x?.name; // undefined
	```
* putting them together:
	```js
		const goodreadsReviews = book.reviews.goodreads?.reviewsCount ?? 0;
		const librarythingReviews = book.reviews.librarything?.reviewsCount ?? 0;
		const totalReviews = goodreadsReviews + librarythingReviews;
	```


## If-Then-Else
* there is a `ternary` shorthand for an `if-then-else` statement. It can be used in places where a regular if-else block cannot (inside a condition or method parameter)
	```javascript
	// standard 'if' block:
	if (name === "Dave") {
		alert("Hey, I know you!");
	} else {
		alert("I don't know you, man");
	}
	// or use ternary:
	function getFee(isMember) {
		return (isMember ? "$2.00" : "$4.00");
	}
	// make the string uppercase or lowercase depending on the flag
	var str = caseFlag ? str.toUpperCase() : str.toLowerCase();
	```
* ternaries can be compounded as much as you like but can become less readable


# Iterations

## For Loops
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

		map.forEach(item => (console.log(item.name + " is " + item.age + " years old.")));
	```
* `while`
	```javascript
		let counter = 0;
		while (counter < 10) {
			counter++;
		}
	```

# Asynchronous Behavior
* a `promise` is a placeholder for a value that will be available in the future
* `async` and `await` are used to handle promises -- they are syntactic sugar used to make async behavior look more like sync behavior and they use promises under the hood
* both promises and async/await provide async non-blocking code execution with options for error handling
* the difference between promises and async/await is the **execution context**
	* the code after a promise continues to execute while the promise is pending -- when the promise resolves, the callback function is added to the microtask queue (job queue) and processed
	* when an async function calls `await`, the function is paused and the execution context is saved -- when the promise resolves, the function is resumed and the value is returned

```javascript
	const url = 'https://jsonplaceholder.typicode.com/todos';

	// using promises
	fetch(url)
		.then(res => res.json())
		.then(data => console.log(data))

	console.log("this will log before data");

	// using async/await
	async function getTodos() {
		const res = await fetch(url);
		const data = await res.json();
		console.log(data);
		return data;
	}
	var data = getTodos();
	console.log("this will still log before data");
	data.then(data => console.log(data)); // but this will log the data when we have it
```

## Promises
* a `promise` is an object that represents the eventual completion (or failure) of an asynchronous operation, and its resulting value
* when the async operation of a promise completes, it will either be fulfilled with a value or rejected with an error
* the states of a promise are:
	* `pending` - initial state, neither fulfilled nor rejected
	* `fulfilled` - meaning that the operation completed successfully
	* `rejected` - meaning that the operation failed

## Async/Await
* `async` is used to declare an asynchronous function
* `await` is used to pause the execution of an async function until a promise is settled
* `await` can only be used inside an `async` function




# The Document Object Model (DOM)

## Loading A Web Page
* when a webpage is loaded, the process looks like this:
	* the page is received as html
	* the document object and all its nodes are stored in memory
	* the rendering engine processes any supplied CSS rules
* when is JS loaded?
	* as the browser loads the document, when it finds a `script` tag it stops and loads it, and does whatever it needs to do
	* if you have in-line script (`document.write('Good afternoon!')`, say) and put it halfway through your document, 'Good afternoon!' will be inserted at that halfway point in the document.

## Nodes
* below the parent `document` object, the DOM consists of a tree of objects called nodes
* there are three types of nodes:
	* elements (`<h3>`)
	* text ("Hello, welcome to Earth!")
	* attribute (css style)

## Access Existing Elements
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

## Adding Elements
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

## Removing/Replacing Elements
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


# Modules
* rather than writing all your js in one script file, you can break it up into modules: smaller, reusable building blocks that can be combined to create a complex application
* you can also import third party modules via package managers like npm
* a module is usually a single standalone file, but not necessarily -- however it is structured, it should be a reusable piece of code that encapsulates implementation details and related functionality (a unit of code) 
* there have been several different ways of writing modules, but ES6 provides an easy way to write modules
* modules can **import** code from other modules -- these are the module **dependencies**
* whatever we **export** out of a module is referred to as the **public API**
* as of ES2022, you can use the `await` keyword outside of async functions -- this is a **top level await** and is only available in modules (`script type="module"`)
	* note that top-level await (awaiting outside of an async function) is blocking, which may or may not be what you want
	* note that if you import a module with top-level await, the importing module will be blocked while the imported module is loading
	* therefore top-level await needs to be used with care as it will block execution within the module itself and in all its dependents

## Module Formats
* regardless of which js version/module pattern you're using, the goal of modules is to encapsulate functionality and expose a public API
* there are several module formats:
	* **IIFE** (immediately invoked function expression) - used before ES6
	* **CommonJS** - used server-side in Node.js
	* **AMD** (Asynchronous Module Definition) - used in RequireJS
	* **UMD** (Universal Module Definition) - a module format that works in both CommonJS and AMD
	* **ESM** (ECMAScript Modules) - the current preferred standard for modules in js
* transpilers like Babel can convert ES6 modules to CommonJS or AMD and vice versa


### IIFE
* before ES6, modules were written in the **module pattern** which used an **IIFE** (immediately invoked function expression) to create a closure and encapsulate the module's code
	* this method uses **closures** to create private variables and functions that are not accessible from outside the module
* this pattern creates issues with bundling and namespace pollution
	```js
		const shoppingCart = (function() {
			const cart = [];
			const shippingCost = 10; // private, not exposed
			const addToCart = function(product, quantity) {
				cart.push({ product, quantity });
			};
			return { cart, addToCart };
		})();
	```

### CommonJS
* **CommonJS** is a module system used in Node.js
* cjs uses `requre` to import modules and `module.exports` to export them
* originally npm was only intended to be used by Node.js on the server, so many npm packages are written in CommonJS (side note: npm packages are either written for ES (browser) or CommonJS (Node), or even hybrid)
* CommonJS syntax can't be executed in the browser, but it's ubiquitous on the server. it will probably eventually be replaced by the ES6 module style, but not quickly, so you'll need to know how CommonJS do
	```js
		// export:
		export.addToCart = function(product, quantity) {
			cart.push({ product, quantity });
			console.log(`${quantity} ${product} added to cart`);
		};

		// how to import:
		const { addToCart } = require('./shoppingCart.js');
	```

### AMD
* **AMD** (Asynchronous Module Definition) loads modules asynchronously, which is useful for the browser -- it is sort of the front end equivalent of CommonJS
* AMD uses `define` to define modules and `require` to import them

### UMD
* **UMD** (Universal Module Definition) is a module format that works in both front and backend
* UMD is more a pattern to configure several different module systems
* UMD works everywhere and is usually the fallback module when ESM doesn't work

### ESM
* **ESM** (ECMAScript Modules) is the current standard for modules in js
* ESM is built into the language and is supported in all modern browsers using ES6 module syntax: `import` and `export`
* better optimization using tree-shaking




## Modules vs Scripts
* **variables**: in scripts, all top-level variables are global, but in modules, they are local to the module -- they must be explicitly exported to be available outside the module
* **strict mode**: modules are always in strict mode -- scripts are in "sloppy" mode by default
* **this**: in scripts, `this` refers to the global object (`window`), but in modules, `this` is `undefined`
* **import/export**: modules can import and export code, but scripts cannot
* **html usage**: modules are loaded with the `type="module"` attribute in the script tag, but scripts are not
* **file download**: whether they're imported in another module or in html, modules are fetched asynchronously. scripts are fetched synchronously (blocking) unless the async attribute is used on the script tag

## Importing
* the `import` statement is used to import functions, objects or primitives that have been exported from an external module
* there are four basic steps here: 
	* **parse** the importing file
	* **download** the imported module
	* **link** the exports from the imported module to the importing file
	* **execute** the code in the importing file
* in order to run our `index.js` file, first it is **parsed** to see what needs to be imported
	* import statements are **hoisted** to the top of the file -- all parsing must be done before any code is executed. imports cannot be done inside functions or conditionals -- they must be known at build time. you can write your import statements anywhere in the file, but they will be hoisted to the top, so that's typically where they are written
	* modules are **imported synchronously** due to top-level (static) imports which makes all imports known before execution. this makes bundling and dead-code elimination possible
* once our file has been parsed and all the imports synchronously known, the imports are asynchronously downloaded
* exports from dependencies are **linked** to the importing file -- the file that uses the imported code does not copy the code, it references it: this allows for code reuse keeps the code DRY
* after the imports are downloaded, the code is executed: first the imported code is executed, then the consuming code is executed

## Exporting
* a **named export** is a way to export multiple values from a module -- you can pick and choose which values you want to import
	* named exports can be made as part of the declaration or separately
	```js
		// named export as part of the declaration
		export const cart = [];
		export const shippingCost = 10;

		// named export separately
		const cart = [];
		const shippingCost = 10;
		export { cart, shippingCost };

		// importing a named export:
		import { addToCart, totalPrice } from './shoppingCart.js';
	```
	* imports and exports can be renamed:
	```js
		// renaming an import/export:
		import { shippingCost as costToShip } from './shoppingCart.js';

		// renaming an export:
		export { cart, shippingCost as sc };
	```
* **namespace objects** are objects that contain all the exports from a module -- you can import the entire module as a single object that then behaves like a class with methods and properties. use a wildcard import statement to create one:
	```js
		import * as ShoppingCart from './shoppingCart.js';
		console.log(ShoppingCart.cart);
	```
* a **default export** is a way to export a single value from a module -- you can import it with any name you want
	```js
		// declaring default export with no name
		export default function (product, quantity) {
			// code
		}

		// importing in another file: give the import any name you wish
		import add from './shoppingCart.js';
	```
	* you can only default import once per file, but you can have a default and named export in the same file. note this is not desireable, but it is possible
	```js
		import add, { shippingCost as costToShip } from './shoppingCart.js';
	```
* note that because module imports/exports are live connections, if you import an object such as an array and modify it, you will see the live changes made to it within the consuming code (i.e. you can just reference your imported `cart.length` rather than needing to import a function that gets the cart count)
	* **so what happens when a module is shared between multiple files?** 


# Building Javascript
* we write code in a modular, easy to manage way, but we need to optimize the code to be consumed by the browser. **building** takes development-centric code and makes it production-ready
* the **build** consists of two steps: **bundling** and **transpiling/polyfilling**
	* **bundling** is the process of combining multiple files/modules into a single file and optimizing it by removing unused code, minifying it, etc
	* **transpiling/polyfilling** is the process of converting modern js code into a backwards-compatible ES5 version
* bundling is done with a **build tool** like webpack, vite or parcel
* transpilation is done with a **transpiler** like babel
* though modern browsers can manage multiple modules, it's still better to bundle for performance reasons in addition to being backwards compatible
* **code splitting** is a technique used to split your code into smaller chunks that can be loaded on demand -- this can improve performance by reducing the amount of code that needs to be loaded initially
* **preloading** is a technique used to load resources before they are needed -- this can improve performance by reducing the time it takes to load resources when they are needed


## Webpack vs Vite
* **webpack** is well-established and has a large ecosystem of plugins and loaders -- it's a bit more complex to set up and configure, but it's very powerful
* **vite** is a newer build tool that is faster and simpler to set up -- it's great for small to medium-sized projects, but it's not as powerful as webpack

## Bundling
* **bundling** is the process of combining multiple files/modules into a single file and optimizing it by removing unused code, minifying it, etc

### Entry Points
* **entry point**: the file where the bundling process starts -- usually `main.js` or `index.js`
* build tools will start building a dependency graph from the entry point file
* SPAs typically have a single entry point, but multi-page apps may have multiple entry points
* types of entry points:
	* **single entry points** are the default and are used for SPAs
	* **"scalable" entry points**: in webpack, object syntax is used to define multiple entry points along with any dependencies
	* **multi-main entry points** combines multiple dependent files together into one "chunk"
* webpack recommends one entry point per HTML file, but there are other ways of defining entry points. see [webpack entrypoint docs](https://webpack.js.org/concepts/entry-points/) and [this](https://bundlers.tooling.report/code-splitting/multi-entry/#webpack) for more info


## Transpiling
* **Babel** is the most common transpiler -- it converts modern js code into a backwards-compatible ES5 version
* many build tools come with Babel built in, but you can also use it as a standalone tool
* Babel accepts plugins and presets to customize the transpilation process
	* you could use an ES3 plugin to transpile to ES3
	* you could use a preset for react or typescript
* not everything can be transpiled -- transpilation is fairly simple replacement for things like simply changing `const` to `var`. more complex features will need to be **polyfilled**

## Polyfill
* a **polyfill** is a piece of code that provides modern functionality on older browsers that do not natively support it
* polyfills are more complex than transpilation operations, including features that are truly new like `Promise` or `fetch`
* when you build your code, you'll see that rather than replacing your code (like changing `const foo` to `var foo`), there are definitions for the missing functions and behaviors added to your code -- the polyfill will add a definition for `Array.prototype.find`
* Babel used to handle polyfills, but now it's recommended to use a separate tool like `core-js` for polyfills
* when importing `core-js` you'll want to just import what you need, such as `core-js/stable`. you may want to further cherry pick what you need by only importing `core-js/stable/array/find` for example
* to polyfill async functions, you'll need to import `regenerator-runtime/runtime` as well
* `modulepreload` is a way to tell the browser to preload a module before it's needed
	* this can improve performance by reducing the time it takes to load resources when they are needed by loading them in parallel
	* however this is not well supported yet so polyfilling can be used to provide this functionality
	* read more about [polyfilling modulepreload](https://guybedford.com/es-module-preloading-integrity#modulepreload-polyfill)

## Production Build
* for a production build, your code will be minified and bundled
* the `<script type="module">` tag you used before could change to a regular `<script>` tag

