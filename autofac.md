# AutoFac

## General
* [Official Documentation](https://autofac.readthedocs.io/en/latest/getting-started/index.html)
* AutoFac provides IoC/dependency injection similar to .NET's built in DI (`Microsoft.Extensions.DependencyInjection` or MEDI) but with some extra features. see [why AutoFac](https://www.mattburkedev.com/why-autofac/) for lots of good examples for how you'd use these features
	* tagged lifetime scopes
	* service metadata/assembly scanning
	* define named/keyed variants of a service
	* resolving factory function 
	* lazy instantiation



## Registration
* in the application configuration, you register your components in a `ContainerBuilder`
* you may register your component as a type or as an interface. usually you would want to expose the type via its interface:
	```csharp
		builder.RegisterType<SomeType>().As<IService>();
	```
* during registration, you may specify different scopes for your services
* once everything is registered to the container, the container must be stored so it can be used to resolve types later

## Scopes
* during execution, you'll need to resolve the components you registered from a lifetime scope
* the container itself is a lifetime scope


## Resolving Services
* you can technically resolve right from the container, but it's not recommended
* resolving directly from the container may result in a lot of things not getting disposed of right away, leading to memory leaks
* the correct alternative to resolving from the container is to create child lifetime scopes and resolve from those
	* at the end of the scope, the child and everything resolved from it gets cleaned up
		```csharp
			using (var scope = Container.BeginLifetimeScope())
			{
				var writer = scope.Resolve<IDateWriter>();
				writer.WriteDate();
			}
		```
* using Autofac integration libraries, this is largely done for you

### Resolving Services in ASP.NET Core
* [AutoFac integration with ASP.NET Core](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html#id6)
* DI is managed by ASP.NET Core -- controller classes are provided with the services you configure in startup

