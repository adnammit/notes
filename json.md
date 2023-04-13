# JSON: Javascript Object Notation
* a very popular method of exchanging data across the web
* commonly corresponds with AJAX
* used by many other languages and programs
	- human-readable
	- hierarchical
	- lightweight way to exchange data -- it's just a string
	- similar to XML but more better
* using an extension like json view or other tools can make it look cleaner
* formatting JSON: you can use **array notation** or **object notation**, and you can combine the two
* **array notation**: just a 'grocery list' of values
	- it's easy, efficient
	- however it takes a lot of overhead to keep track of what each value means
	- example: `['string', 423, true, [1,2,3]]`
* **object notation**: uses property-value pairs
	- the name of a property and the value of that property. example: `"name": "Smith"`
	- good for more complex objects
* example of a simple JSON file that combined object and array notation:
	```json
	[
		{
			"name": "Jim",
			"age": 20,
			"phone": "502-214-4243",
			"cars":["Peugeot"]
		},
		{
			"name": "Felicia",
			"age": 32,
			"phone": "502-214-4243",
			"cars":["Audi", "BMW", "Fiat"]
		},
		{
			"name": "Maude",
			"age": 56,
			"phone": "502-214-4243",
			"cars":["Chevy", "Jeep"]
		}
	]
	```
	- note that in properly formatted JSON object notation, both the property and its value must be surrounded in double quotes
	- if a JSON file isn't properly formatted, you'll usually end up with a JS error
	- to find out if it's properly formatted or not, use an online validator like [JSONlint.com](JSONlint.com)

## Serialization and Deserialization in C#
* [microsoft doc](https://learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/how-to?pivots=dotnet-6-0#serialization-behavior)
* **serialization** is sometimes referred to as **encoding** and means to turn an object into a string
* **deserialization** is sometimes referred to as **decoding** and means to turn a string into an object

### Deserialization to .NET Objects
* `JsonSerializer.Serialize`
* can be synchronous or async

#### Default Behavior
* *all public properties are serialized*. you can specify which properties to ignore with `[JsonIgnore]`
* the default encoder escapes non-ASCII characters, HTML-sensitive characters within the ASCII-range, and characters that must be escaped according to the RFC 8259 JSON spec
* json is minified but can be prettified with the `WriteIndented` option
* casing will match the .NET names but can be customized
* circular references are detected and throw exceptions. configuration can be added to preserve and handle circular references
* fields are ignored but can be included with the `IncludeFields` option
* order of serialized properties can be customized (why is this useful again?)
* supported types:
	- .NET primatives that map to js primatives such as numeric types, strings, and Boolean
	- user-defined CLR objects (POCOs)
	- one-dimensional and jagged arrays
	- collections and dictionaries from select namespaces
	- custom converters can be added to handle additional type conversions

### Deserialization to .NET Objects
* to read json, create a class with properties and fields that match the json structure
* `JsonSerializer.Deserialize`
* can be synchronous or async
* any json properties that are not represented in the class are ignored
* if any properties on the type are required but not present in the json payload, deserialization will fail (?????)
* **COOL TRICKS!**
	- if you only need a subsection of the json and don't have a full class to represent the whole payload, you can deserialize into a **JSON DOM** and then extract what you need from the DOM
	- VS2022 can automatically generate a class from json! create a new class file and delete the template code, then `Edit > Paste Special > Paste JSON as Classes`

#### Default Behavior
* property name matching is case-sensitive but can be configured
* if json contains data for a read-only property, the value is ignored and no exception is thrown
* non-public constructors are ignored
* immutable types with no public `set` can be deserialized to with `[JsonConstructor]` attribute
* enums are supported as numbers but they can be serialized as strings (and then deserialized to strings)
* fields are ignored but can be included with the `IncludeFields` option or `[JsonInclude]` attribute
* comments and trailing commas in the json throw exceptions but can be handled
* default max depth for deeply nested json is 64


### Summary of .NET Serialization Gotchas
* fields and private properties won't be handled unless you handle them
* when deserializing, values for read-only properties will be ignored and no exception will be thrown (sneaky!)
* casing must match unless otherwise specified
* circular references, comments and trailing commas will bork your process unless you handle them
* deep json may not completely transfer (default max depth 64)



