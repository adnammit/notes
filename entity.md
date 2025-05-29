# Entity Framework

## Overview
* Entity Framework (EF) is an open-source object-relational mapping (ORM) framework for .NET applications
* enables developers to work with databases using .NET objects, eliminating the need for most of the data-access code that developers usually need to write -- i.e. it handles the mapping of objects to database tables and vice versa with a bit of coding, rather than having to manually map out each sql variable/column
* entity works well with LINQ 
* EF Core is the latest version of Entity Framework, designed to be lightweight, extensible, and cross-platform


## Vocabulary
* **navigation properties**: navigations are properties in entity classes that hold references to other entities, allowing you to navigate relationships between entities. navigations may be a single entity or a collection of entities (zero to many)
* **shadow properties**: when an entity has a relationship with another entity, it has a navigation to that object -- in addition, you may add an FK identifier property, but if you don't, EF will add a shadow property to represent the FK:
	```csharp
		public class Order
		{
			public int Id { get; set; }
			public string Name { get; set; }
			// FK - would be filled in as a shadow prop, not needed:
			public int CustomerId { get; set; }
			// navigation property:
			public Customer Customer { get; set; }
		}
	```
* **context**: you will need to write a bit of db context code that will derive from `DbContext` which will hold references to/manage `DbSet` properties
* **migration**: a migration is a way to update the database schema to match the current model in code. migrations can create tables, add columns, modify existing columns etc. they can be used only for initial creation or to continuously update as your model changes 
* **scaffolding**: scaffolding works in reverse of migrations - it is a way to generate code based on an existing database schema. it can be used to create entity classes, razor pages, and a db context class that matches the schema of the database. when doing this, you should use partial classes and distinct namespaces to keep your business logic separate from your data access logic so it doesn't get overwritten when scaffolding


## Code-First Mapping
* [resource](https://learn.microsoft.com/en-us/ef/ef6/modeling/code-first/data-annotations)
* EF will infer much of the mapping between your objects and the database schema, but you can also use **attributes** or **fluent API** to configure the mapping
	* using attributes is the most common way to configure mapping
	* Fluent API is a way to configure the mapping using imperative code, rather than attributes. you can do everything with Fluent API that you can do with attributes and more
	* [attributes vs FluentAPI](https://stackoverflow.com/questions/5354900/entity-framework-code-first-advantages-and-disadvantages-of-fluent-api-vs-data)
* however you do it, your columns/properties should explicitly be either nullable or non-nullable
	* if you have a non-nullable column in your db that is represented by a non-nullable property in your class, you can use the `Required` attribute

### Attributes
* you can use attributes such as `Key`, `Column`, `Required`, `MaxLength`, `MinLength` etc to configure how your object is mapped to the db schema
* EF will infer a lot -- you don't necessarily *need* a `Key` attribute on your primary key property - EF will infer the key by looking for the property `Id` or `{className}Id` and if your PK isn't named that, you'll need to use the `Key` attribute
* `NotMapped`: if you have a property that you don't want to be mapped to the db, you can use the `NotMapped` attribute. for example, a computed property that is calculated based on other properties should not be mapped to the db

