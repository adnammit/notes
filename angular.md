# Angular
* [quick start](https://angular.io/guide/what-is-angular)
* much like Vue or React, Angular is a front-end framework for building single page applications (SPAs)
* Angular is built entirely in typescript so ts is a first-class citizen and not an afterthought
* Angular includes:
	* a component-based framework for building scalable web applications
	* well-integrated libraries with a variety of features including routing, form management, and client-server communication
	* a suite of dev tools to build, test, deploy and update code

# Basic Concepts
* **components** are UI building blocks which are used to generate the DOM
* **bindings** are used to connect components to the DOM
* **decorators** are used to add configuration metadata to a class, its members, or its method arguments. the metadata determines how the component, class, or function should be processed, instantiated and used at runtime
* **first-party libraries** expand the functionality of the framework and include routing, forms, httpClient, animations, PWA, and schematics
* **dependency injection** is wired into the Angular framework. classes with angular decorators can configure the dependencies they need
* the [Angular CLI](https://angular.io/cli) helps with development tasks

## Components
* **components** are declared using a `@Component` decorator
* components consist of
	* css **selector** which defines how the component is used in a template. html elements in the template that match the selector become instances of the component
	* an html **template** that instructs Angular how to render the component
	* optional set of css **styles** that define the look and feel of the component
* components offer strong encapsulation and improve readability and reusability

## Templates
* can be supplied inline (`template`) or in a separate file (`templateUrl`)

## Bindings
* bindings are achieved in one of several ways: interpolation, property binding, and event handlers
* **string interpolation** (`{{ title }}`) allows you to insert dynamic content from your component into the HTML template
* **property binding** (`[id]="myComponentId"`) allows you to set the values of properties and attributes of HTML elements
* **class and style binding** is a more complex version of property binding with a few different rules -- [read more](https://angular.io/guide/class-binding)
	* a **single class** can be conditionally set by binding to a truthy expression
	* **multi-class** bindings are specified using a class expression which can take several forms:
		* string: a space-delimited string of class names
		* object with class names as keys and truthy expressions as values
		* array of class names as strings
		```html
		<!-- string -->
		"modal-title modal-text-emphasized text-danger"
		<!-- object -->
		{ modal-title: isTitle, modal-text-emphasized: true, text-danger: hasError }
		<!-- array -->
		[ 'modal-title', 'modal-text-emphasized', 'text-danger' ]
		```
* **attribute binding** (`[attr.colspan]="calculatedColspan"`) allows you to set the value of an attribute for an HTML element
* **event listeners** (`(click)="showConfirm()"`) allow you to listen for events and respond to them
* **two-way binding** (`[(ngModel)]="name"`) allows for event listening and for data to be shared between parent and child components (see below)
* examples:
	```html
	<!-- property binding to control the disabled attribute: -->
	<button [disabled]="!canClick">Click me!</button>
	<!-- interpolation and property binding together: -->
	<p [style.color]="fontColor">{{ message }}</p>
	<!-- event listener to respond to a click event: -->
	<button (click)="showConfirm()">Confirm</button>
	<!-- style binding and conditionally add the single 'active' class -->
	<input [style.borderStyle]="borderStyle" [class.active]="isActive" />
	<!-- dynamically set classes with a class expression -->
	<div [class]="classExpression"></div>
	<!-- two-way binding -->
	```

### Two-Way Binding
* [documentation](https://angular.io/guide/two-way-binding)
* as the syntax suggests, two-way binding combines property binding to set a child element's property and event binding to listen for changes
* two-way binding requires use of the `@Output` and `@Input` decorators

## Directives
* [**directives**](https://angular.io/guide/built-in-directives) are classes that add additional behavior to elements
* directives can manage what users see, including forms, lists, and styles
* the most popular directives are `*ngIf` and `*ngFor`


## Dependency Injection
* [DI documentation](https://angular.io/guide/dependency-injection)
* classes with Angular decorators (such as components, directives, pipes and injectables) can configure the dependencies they need
* the angular DI system is comprised of two main roles: **consumer** and **provider**
* **provider** classes use the `@Injectable` decorator to show that the class can be injected
* **root** vs **component** level registration: 
	* **root** level registration: a single instance is shared across the application
	* **component** level registration: a new instance is created for each instance of the consumer component

