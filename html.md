# HTML ANATOMY:
* **tag**: a demarcation which tells the browser to display the specified
  object. tags are building blocks of html
	- tags may or may not have attributes
* **elements**: have an opening and closing tag
* "**singleton**" or "void elements" have only one tag. ex: <br>
* **attribute**: a characteristic of a tag, contained within the same <>
	- usually consist of a key/value pair
	- ex: in <img src="happy.jpg" alt="happyface">, `img` is the tag, `src` and `alt`
	  are attributes
* **entity**: some symbols need extra help to be displayed.
	- ex: &copy; displays the copyright symbol
* basic structure of an html document:
	```html
	<!DOCTYPE html>
	<html>
		<head>
			<meta charset="utf-8">
			<title>This is the TITLE!</title>
		</head>
		<body>
			<header>
			</header>
			<section>
			</section>
			<footer>
			</footer>
		</body>
	</html>
	```

# BASICS:
* relative vs absolute URLs
	- when linking to your own pages, it's recommended to use relative
	  links so that dirs can be rearranged without having to rewrite code
	- use absolutes for referencing external material
* "Mobile-First development"
	- it makes sense to develop the mobile version first, and then add
	  complexity for the desktop version

# IMG:
* alt attributes are required in case the photo can't be loaded, but
  also so that search engine spiders and screen reading software have
  data to read

# HEXADECIMAL:
* base-16 numbering system:
	- 0123456789ABCDEF
	- for a two-character number (E7, 9A, 00, FF, etc) that gives us
	  256 possible values
* the first two characters in a six-digit hex value represent red, the
  second two represent green, and the last two represent blue
* the higher the number, the more intense the chroma
* for example, #000000 is black; #FFFFFF is white
	- if all numbers are the same (#888888) it will be some element of
	  grayscale
	- #00FF00 would be the max amount of green
* shorthand: you can use three digits instead of six if the three pairs
  are the same (#FFF == #FFFFFF; #3AD == #33AADD)

# CLASSES VS IDS:
* both are 'hooks' -- identifiers we need to be able to manipulate
	individual web elements
* ids are unique:
	- each element can have only one id
	- each page can have only one element with that id
* classes are not unique:
	- each element may have multiple classes
	- you may have multiple elements with the same class on the same
	  page
* an element can have both an id and class(es)
* why even use ids then? ids have one special browser functionality:
	- ids can be given as a hash value in a url:
		http://www.mysite.com#comments
	- to css, there is no difference between ids and classes
	- JS cares: jQuery manipulates the hell out of classes, but it's
	  not a good idea to go mucking around with ids
* moral of the story: use classes unless you really need an id



