# CQRS (Command and Query Responsibility Segregation)

## Quick Links
* [documentation](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs)
* [CQRS & DDD](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/)
* [When to use CQRS](https://martinfowler.com/bliki/CQRS.html)

## The Problem
* traditionally, the same DTOs are used for all CRUD operations
* In practice, the DTO for an update or delete operation may include far less information than a read operation
* Writes may require complex data validation

## The Solution
* Where these pathways are significantly different, CQRS can be used to separate updates into **commands** and reads into **queries**

## When To Use It
* CQRS isn't always the correct solution -- it can be risky splitting out a conceptual model into two separate pieces -- you have to mentally combine two different things to have a complete understanding of how something works. it also sounds hard to test?
* CQRS works well in **event-based models**/event collaboration/event sourcing
* CQRS works well with **eventual consistency**
* CQRS can work in DDD when implementations are contained within a bounded context -- that is, it is encapsulated within the context such that each bounded context can dictate its own implementation
