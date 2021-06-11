# PHP

* PHP stands for PHP Hypertext Preprocessor
* PHP is:
    - PHP is a server-side scripting language
    - PHP is object oriented
    - exceptions and error handling
* PHP can be embedded directly into html, or created as a standalone .php
  file that is executed from a cli
* for example, when php is embedded in an html document, the client
  computer reads the php which generates a request to a server which
  processes the script and returns html to the client machine
    - if PHP is embedded, it might be strewn throughout the document,
      or it might be mostly a large block of script and data processing
      right at the beginning.

## SYNTAX:
* a simple php block looks like:
    <?php echo "Hello world!"?>
    // this is a single line comment
    # this is also a single line comment
    /*
       this is a multi line comment
    */
* the php block is delineated with <?php ... ?>
* if you are using a single line of code within a php block, you don't
  have to follow it with a semi colon, but if you contain multiple lines
  in the same block, they should be semi-colon separated.
* escape sequences:
    - you can't use a <br> (or other html) inside php, so use an escape
      sequence like \n

## VARIABLES:
* a variable name always begins with a $ and then an underscore, letters,
  or letters, underscores and numbers combined
* a variable declaration looks something like:
    $name = "Mike";
* variable names should use underscores rather than
  upper/lowercase to describe lenghty variable names
    - eg calculate_payout rather than calculatePayout
* arrays:

## DATA TYPES:
* need to check your variable type? use:
	<?php echo gettype($foo); ?>
* INTEGERS
* FLOATS
* STRINGS
    - similarity with arrays - index access
    - can be denoted with '' or ""
* BOOLEAN
    - isset($bool) gives a true/false return in code
    - 0 and 0.0 are false, as well as empty strings and arrays
    - isset doens't necessarily give us a t/f answer but we can use
      type casting to get it.
* ARRAYS
    - arrays can hold different types of data!
    - array indices are 0-based (first element is 0)
    - arrays are moderated with a key/value pair
    - KEY: the index number
    - VALUE: the data stored at that key index
    - accessing a single element:
	$names{3};  // gives the 4th element
    - remember that strings are arrays also -- you can use the same
      techniques for altering/pulling data from strings
    - declaring an array using the following syntax:
	$names = array('Mike', 'John', 'Barb', 'Guido');
    - shorthand method for declaring an array:
	#array_example = [];
    - print out all values of an array with:
	<?php print_r($names); ?>
    - string elements in an array can be denoted with '' or ""
    - alter an element in an array with:
	$array_foo[2] = 'bar';
    - add an element to an array with:
	$array_foo[] = 'baz';
* ASSOCIATIVE ARRAYS:
    - instead of an automatically generated numerical key, each element of
      the array is paired with a corresponding value
	* for example, maybe unique names would be the keys and non-unique
	  eye colors would be values
    - accessing an element:
	echo $eyecolors['Greta'];
    - adding or modifying an element:
	$eyecolor['Fiona'] = 'Amber';
* OBJECTS
* CONSTANTS:
    - variables change -- constants do not. good for setting up program
      parameters
    - don't require a dollar sign designation and are case sensitive like
      variables (but you'll want to use all-caps, of course)
    - constants are listed at the top of each document within a php block
    - PHP has built in constants
    - syntax:
	define("YEAR", 2014);
	echo YEAR;

## VAR DUMP
* displays the contents of a variable on the screen

## TYPE CASTING:
* syntax: cast the variable foo (for the purposes of this funciton call)
  as a bool and dump it:
    var_dump((bool) $foo);
* hard cast:
    int $foo = 1;
    string $bar = "1";
    bool $baz = FALSE;

## PHP OPERATORS:
* here are some operators:
    +, -, =, *, /, +=,
    .=  string assignment
* autoincrement/autodecrement: ++, --
    $product = $product + 1;  //OR:
    $product++;
* comparison operators
    ==, !=, <>
    >, <, >=, <=
    ===, !==  ternary operators
    - example:
	var_dump($a == $b);
    - == vs ===
	* == : "is it equal?" according to php, 10 == '10'
	* === : "is it identical?" 10 === '10' is false
* concatenation operator: uses a dot:
    "hello " . "world!";
* logical operators:
    and (&&), or (||), !
	var_dump($a and $b); //returns true if a and b are true
	var_dump($a or $b); // returns true if at least one is true
	var_dump(!$a); // returns T if boolean a is F

## CONDITIONAL STATEMENTS:
* an if/else block follows the following syntax:
	```php
	if(...){
	    // run some code
	}elseif(...){
	    // run some code
	}else{
	    // run some code
	}
	```

## LOOPS:
* for()
	```php
	for($counter=0; if $counter < 10; $counter++){
	    echo 'I can count to ' . $counter;
	}
	```
* while()
* foreach()
    - really powerful. allows you to iterate through an array (etc) to
      access specific data, rather than just counting
* do...while



## OBJECT ORIENTED PROGRAMMING IN PHP
* objects are instances of classes
* PHP now fully supports the OOP schema
* major concepts:
    - encapsulation
	* controlled access
	* makes constructs extensible
    - inheritance
	* inheritance allows reuse of code
    - polymorphism
	* shared methods with different implementations across classes
* each class contains variables (properties) and functions (methods)
* object operator: ->
	$p->name; // accesses the $name property of $p's class
* PHP uses 'this' just like Java, but it's $this

SCOPE:
* both properties and methods have scope
* public
    - accessible anywhere
    - the 'var' keyword also makes a variable public
* protected
    - accessible within the class itself, and also parent and child
      classes
* private
    - accessible only with the class

## CONSTRUCTORS:
* constructors do not have a scope declaration.
* syntax for creating a new product with three properties:
	function __construct($name, $price, $desc){
	    $this->name = $name;
	    $this->price = $price;
	    $this->desc = $desc;
	}
* here's a child constructor that calls the parent and sets its own
  property value:
	function __construct($name, $price, $desc, $flavor){
	    parent::__construct($name, $price, $desc);
    	    $this->flavor = $flavor;
	}

## EXCEPTIONS
* exceptions are thrown through tries and catches
* a 'try' must have at least one corresponding 'catch'
* a try may have multiple catches -- a catch block
* we can use multiple catch blocks to catch multiple exceptions
* if an exception is thrown, the code following the catch block will not
  be executed
    - this behaviour can be modified with 'set exception handler'
* the exception thrown must be a member of php's exception class or a
  subclass.
    - otherwise you'll get a fatal error
* pseudo code example:
	try{
	    // run some code that might fail
	} catch(Exception $e){
	    // inspect $e
	}
* php's exception class has some public methods that can help us find our
  error:
	$e->getMessage();   // returns error message
	$e->getCode();	    // returns integer code for error type
	$e->getFile();	    // which file the error occurred in
	$e->getTrace();	    // multidimensional array of file, method,
			    //	class, and arguments
	$e->getTraceAsString();
			    // same as 'getTrace' but in a string
	$e->__toString();   // returns a string that describes exception

## TOOLS AND TIPS FOR PHP OOP:
* method_exists: test whether or not a class method exists. returns a bool
  and takes a class or class object as the first arg, and the function
  name as a string for the second arg.
	method_exists("Product", "getPrice");
    or:
	method_exists($grape_soda, "getPrice");
* is_subclass_of: checks to see if a given object has a parent of a given
  class name. like method_exists, it returns a bool and takes either a
  class name or instance of a class as the first arg
	is_subclass_of("Soda", "Product"); // is product a parent of soda?
	is_sublcass_of($grape_soda, "Product");
* you can use strings to reference classes and functions!
    - you can use a variable name in place of a product name:
	$class = "Product";
	$p = new $class;
    - you can use a set a variable to a string of a function name:
	$m = "getName";
	$name = $p->m(); // note: no $ and add () here

## SECURITY
* SQL injection: a malicious user injects SQL codes as input to alter or
  dump sensitive data
* protection: "filter input, escape output"
* to protect against malicious input, input should be filtered
    - using 'prepare' and 'binding'
* using 'prepare' and then 'execute'
    - ensures that only the statement we want gets executed


## PHP AND DATABASES: PDO's
* a PDO is a PHP Data Object gives us a standardized way of working with
  databases
    - first we need a PDO driver
    - out object is an instance of the PDO class, having built in methods
      and properties
    - we should surround all of our database work with try/catch blocks
    - we also need an Exception class object to catch exceptions
* surround your var_dump output with <pre> to make it pretty:
	echo('<pre>'); // format the results a little more nicely
        var_dump($results->fetchAll());
	echo('</pre>');

# DEVELOPING WEBSITES IN PHP

## ENVIRONMENT
* see stephen's notes for running php locally within your project's dir

## MVC: MODEL VIEW CONTROLLER METHODOLOGY
* this is a strategem for building code
    - the view includes html, css and the form that the user interacts
      with
	* user interaction code
    - the model is comprised of the data that is needed and generated
	* the database
	* database-related code
    - the controller validates information and interfaces between the
      user and the database(model)
	* links the view code to the model code
* how does this relate to our website?
    - view the front page as a jumping-off point for all other
      processes and responses
    - everything that comes in from the outside must go through the
      front page controller

## COMPOSER
* manages package dependencies for php projects
* use:
    - while in your working directory, type 'composer require'
    - enter the package to search for
    - composer installs it and adds the package to the .json file

## INTRODUCING THE SLIM FRAMEWORK
* a framework is a software structure that provides generic functionality
  that we can modify to our own needs
    - slim provides this generic functionality for a website application
      or API (application programming interface)
    - if we need more, we can add code to it to add more, like emailing
      or templating languages
* we only need a microframework (like slim) for our modest site -- other
  projects might require heavier lifting
* by default, slim provides:
    - routing
    - http request/response
    - caching
    - middleware
    - sessions
    - overkill crypto

## CREATING A NEW APP OBJECT
* create a new object with:
	$app = new \Slim\Slim();
* then use the 'get' function to set up our page
	$app->get('/', function(){};
    - the first arg is our page ('/' for the home page)
    - 'function' is our anonymous closure that we use to make
      functionality
	* function may take a single arg, or none.
* 'render' allows us to project an html doc into another page
    - render looks in the templates folder for the corresponding document
    - the variable ($app for example) must be passed in for us to render
      the template in $app

## TWIG: TEMPLATE LIBRARY FOR PHP
* twig allows for clean coding using template-oriented syntax
* allows us to insert a content block into a stock skeleton page

## SLIM: NAMED ROUTING:
* we can use slim to dynamically generate URLs
* using the $app->get() function to name a route, we can then navigate to
  that page using the urlFor() function:
    // setting up the named route:
	    $app = new \Slim\Slim();
	    $app->get('/hello/:name', function ($name) {
	        echo "Hello, $name!";
	    })->name('hello');
    // and the link:
	<a href="{{ urlFor('hello') }}" class="selected">Hello!</a>

## SETTING UP THE FORM:
* the form is created on the contact page using html and the routing is
  set up through the index page.
* security: never trust user input
* sanitaization of this form consists of:
    - make sure there are no weird characters
    - make sure email is an email
    - make sure there is not null data
* using php's filter_var:
    - allows us to strip tags, code, special chars, etc
    - special filtering for strings vs email, etc

## SWIFTMAILER:
* there are several ways of sending email but using swiftmailer has more
  functionality, error handling, etc
* we will use composer to install the swiftmailer package
* using swiftmailer:
    - we'll need a transport and mailer object instantiation
    - then we need to compose the email


