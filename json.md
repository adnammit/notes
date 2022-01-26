# JSON: JAVASCRIPT OBJECT NOTATION
* a very popular method of exchanging data across the web
* commonly corresponds with AJAX
* used by many other languages and programs
	- human-readable
	- hierarchical
	- lightweight way to exchange data -- it's just a string
	- similar to XML but more better
* example:
	teamtreehouse.com/amandaryman.json
	- using an extension like json view or other tools can make it look cleaner
* formatting JSON: you can use ARRAY NOTATION or OBJECT NOTATION
	- you can combine both
* array notation: just a 'grocery list' of values
	- it's easy, efficient
	- however it takes a lot of overhead to keep track of what each value means
	- example:
	['string', 423, true, [1,2,3]]
* object notation: uses property-value pairs
	- the name of a property and the value of that property. example:
	name: 'Smith'
	- good for more complex objects
	- example of a simple JSON file:
	[
		{
		"name": "Jim",
		"age": "20",
		"phone": "502-214-4243"
		},
		{
		"name": "Felicia",
		"age": "32",
		"phone": "502-214-4243"
		},
		{
		"name": "Maude",
		"age": "56",
		"phone": "502-214-4243"
		}
	]
	- note that in properly formatted JSON object notation, both the property and its value must be surrounded in double quotes
	- if a JSON file isn't properly formatted, you'll usually end up with a JS error
	- to find out if it's properly formatted or not, use an online validator like JSONlint.com
