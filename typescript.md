# Typescript

## Resources
* use the playground and check out docs on [typescript lang](www.typescriptlang.org)


## Best Practices
* [official best practices](https://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html)
* [more best practices from AWS](https://docs.aws.amazon.com/prescriptive-guidance/latest/best-practices-cdk-typescript-iac/typescript-best-practices.html)
* don't ever use `String`, `Number`, `Boolean` or `Object` types
* `string` vs `String`: TL;DR: always use `string`
	* `string` is a primative type and `String` is an `Object` type
	* `typeof` checks will not behave as expected for `String` types -- they're objects
	* strict equality checks of `string` vs `String` will not match
* use `unknown` instead of `any`
	* if you're using `any` you're probably doing it wrong -- use types!
	* `any` should only be used when in the process of migrating js -> ts
	* `any` is effectively the same as using `ts-ignore` everywhere
	* `unknown` is a type-safe alternative to `any`
* in a similar spirit, use strict in your tsconfig file to keep yourself honest
* use `let` and `const` where appropriate -- don't overuse `var` which is global when declared outside functions and can be unknowingly redeclared
* coming from C#, you will want to use interfaces but you should probably be using a type alias instead -- see Type Aliases vs Interfaces above


# What The Hey Is It
* ts is a superset of js that compiles to js
* it is cross-browser and cross-platform compatible
* it's open source
* benefits of ts over js:
	* ts uses optional static typing and a type inference system (the type of a var with no type in its declaration can be inferred from its value)
	* ts supports OOO concepts like inheretance and classes
	* js is an **interpreted language** so errors are caught at runtime. since ts is compiled, some errors are caught at compile time
* cons of ts:
	* requires more overhead
	* less flexible
	* not always necessary
* tooling and support
	* can be used directly with node.js
		* `npm install -g typescript` will allow you to run `tsc foo.ts`
	* Visual Studio
		* autocompiles on save
		* adding a new ts file generates a new module with a sample class and interface
		* VS extension "Web Essentials" includes a preview of your js
	* typescript playground allows you to type ts and see the generated js immediately with intellisense

## TS vs JS
* js can feel messy, which leads to difficulty maintaining and structuring
* there is overhead, so you don't always need to bother with it, but it's scalable and good for enterprise solutions
* js provides a dynamic type system
	* that's nice -- variables can hold any object, types are determined on the fly
	* however it's difficult to ensure proper types are being passed
	* using ts enforces better practices (don't have to worry that everyone used `===`)
* devs migrating from server to client side apps can handle ts better
* ts supports interface definitions, which js does not natively support

## Features
* support for js -- you don't have to rewrite everything, you can mix-and-match
* static typing allows you to catch errors during compilation
* encapsulation through classes and modules
* support for construtors, properties and functions (the usual)
* lambda funcs
* intellisense support

## Compiler
* you can use the command line tool `tsc` to manually compile ts
* visual studio automatically generates a corresponding `.js` file when you save a `.ts` file
* so this nice legible typescript:
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
* gets compiled into this javascript:
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

## Syntax And Keywords
* ts follows the same basic syntax rules as js
	* {} defines code blocks
	* semicolons end code expressions
	* js keywords like `for`, `if` etc
* keywords
	* `class`: container for members
	* `constructor`
	* `function`: do not use the keyword inside classes, but do use it outside
	* `exports`: export a member from a module
	* `extends`: extend a class or interface
	* `implements`: implement an interface
	* `interface`: contract that can be implemented by other types
	* `module`/`namespace`: container for classes and other scoped code
	* `public`/`private`: member visibility modifiers
	* `...`: rest parameter syntax
	* `=>`: callback functions, return type of a function
	* `<typename>`: casting/conversion
	* `:`: separator between variable/parameter names and types
* hierarchy
	* use of namespaces, modules and classes removes things from the global namespace
	* a **module** is a container that exports code. it contains both classes and interfaces
	* **class**/**interface**
		* fields, constructors/properties/functions

# Types, Interfaces and Classes

## Types, Variables And Functions
* when using the `var` keyword, typescript uses **type inference** (like js and csharp) wherein it infers the type of the variable based on the assignment
* **type annotation** can be used to explicitly set the type
	```ts
		// using type inference:
		var num = 2;
		// using type annotation:
		var num: number = 2;
		// ambiguous type, no value
		var foo;
		// var with no value, but we know the type
		var foo: number;
		// type inference with two different types (casts number to string)
		var bar = num + ' is my favorite number.';
		// nope
		var notHappy: number = num + ' is unlucky.';
	```
* `any` is the base type in typescript -- the default type of a `var` when the type is ambiguous. but it should only be used very sparingly


## Interfaces
* unlike C#, typescript interface names should not be prefaced with `I` as their usage is more broad
* interfaces can be merged via **declaration merging**: two different interfaces with the same name are automatically merged into one
* interfaces can be **extended** to promote interface segregation
* interface members must be statically known at compile time -- they cannot include union types

## Type Aliases
* types are used to define the shape of an object and the signature of a function
* types refer to primitives like `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `object` but we can also roll our own type definitions using **type aliases**
* **type aliases** mean "a name for any type" -- you don't really create a new type, you just create a new name for an existing type
* type aliases are useful for complex types that are used repeatedly, and for self-documenting code
	```typescript
		type User = {
			id: number;
			name: string;
			email: string;
		};

		// DataType can only be one of these three strings
		type DataType: string = 'json' | 'xml' | 'csv';
	```
* **union types** can document that a variable can be one of several types
	```typescript
		type ErrorCode = string | number;
	```	

## Interfaces vs Type Aliases
* type aliases and interfaces accomplish more or less the same thing
* both type aliases and interfaces can be implemented by classes, with the caveats below
* [more about aliases and interfaces](https://blog.logrocket.com/types-vs-interfaces-typescript/)

### When to Use a Type Alias
* you need to type a primitive -- interfaces cannot be used to type primitives
* you need to declare a union type -- interfaces cannot be used to declare union types
* you want to type functions more easily/with more readability
* type aliases can also be used with mapped types, conditional types, type guards, and other advanced type features that are not supported by interfaces
```typescript
	// primitive type alias
	type Address = string;

	// primitive union
	type NullOrUndefined = null | undefined;
	type Transport = 'Bus' | 'Car' | 'Bike';

	// interface union
	interface CarBattery {
		voltage: number;
		ampHours: number;
	}
	interface Engine {
		cylinders: number;
		horsepower: number;
	}
	type HybridCar = Engine | CarBattery;

	// function type alias
	// TODO: simpler example
	type CarType = 'EV' | 'Petrol';
	type ChargeEV = (kws: number) => void;
	type FillPetrol = (type: string, liters: number) => void;
	type RefillHandler<A extends CarType> = A extends 'Petrol' ? FillPetrol : A extends 'EV' ? ChargeEV : never;
	const chargeTesla: RefillHandler<'EV'> = (power) => {
		// Implementation for charging electric cars (EV)
	};
	const refillToyota: RefillHandler<'ICE'> = (fuelType, amount) => {
		// Implementation for refilling internal combustion engine cars (ICE)
	};

```

### When to Use an Interface
* you need to alias an object type -- interfaces cannot alias primitives
* you need to merge your declarations: interfaces can be merged, type aliases cannot
* you need to extend your types: interfaces can be merged, type aliases cannot be merged, but can be extended using the intersection operator. useful when writing or consuming a third-party library where the types may be extended. limitations of extension:
	* interfaces can extend type aliases with the exception of union types because the members are not statically known -- the interface definition must be statically known at compile time
```typescript
	// type merging
	interface Client {
		id: number;
	}
	interface Client {
		name: string;
		email: string;
	}
	const client: Client = {
		id: 1,
		name: 'John Doe',
		email: 'test@test.com'
	}

	// type extension
	interface VipClient extends Client {
		loyaltyPoints: number;
	}

	// compare to type extension using type aliases and the intersection operator
	type VipClient = Client & { loyaltyPoints: number; }
```


## Classes And Inheritance
* base and derived classes are often called super and sub classes in ts
* use the `extends` keyword to derive a subclass from a base class
* use the `super` keyword to call the base method
	* note that all subclass constructors *must* call `super()`
* make the constructor of the base class protected if you want to restrict instantiation of this class -- only instances of its extended classes can be created
* use the `instanceof` keyword to determine if an object is an instance of a given class
	```csharp
		class Animal {
			name: string;
			constructor(theName: string) {
				this.name = theName;
			}
			move(distanceInMeters: number = 0) {
				console.log(`${this.name} moved ${distanceInMeters}m.`);
			}
		}

		class Snake extends Animal {
			constructor(name: string) {
				super(name);
			}
			move(distanceInMeters = 5) {
				console.log("Slithering...");
				super.move(distanceInMeters);
			}
		}

		let animal: Animal;
		animal = new Snake('Jerry');
		animal instanceof Snake; //true
	```

### Abstract
* `abstract` classes can be created that other sub classes are derived from
* abstract classes cannot be directly instantiated
* abstract classes can be references
	```typescript
		let myClass: AbstractClass;
		myClass = new DerivedClass();
	```
* abstract classes can contain concrete methods and/or abstract methods
* abstract methods *must* be implemented in derived classes
* access modifiers for abstract classes and methods are optional
*


# Modules


## Library Typing
* you can use third-party js libraries in ts -- ts is just a superset of js after all -- but you'll get an error if you try to use a library that isn't typed (a typed library includes a type definition file)
* if a module is not typed, the ts community may have provided a typing module in the [Definitely Typed Repository](https://github.com/DefinitelyTyped/DefinitelyTyped) that can be installed via `npm` via something like `npm install @types/my-untyped-module` 
* you can also write your own **type definition file** and add it to the "include" section of your `tsconfig.json`



# Troubleshooting

## Typescript In The Browser
* is your `ts` file not refreshing when you're using chrome devtools?
	* with devtools open, do a `Empty Cache and Hard Reload` by right-clicking on the reload button

