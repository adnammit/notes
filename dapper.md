# Dapper
Dapper is a simple object mapper for .NET that provides a way to map database results to .NET objects. It is a lightweight and fast alternative to Entity Framework, which is a more feature-rich ORM (Object-Relational Mapper).

## Dapper vs Entity
* Dapper is a micro ORM, while Entity Framework is a full-fledged ORM
* Dapper requires you to actually know SQL - EF is kind of it's own thing
* Dapper is easier to debug than EF: 
	* EF creates queries dynamically, so if you're trying to debug performance on the db server, it's hard to know what is making the query
	* it's also hard to get visibility into what is being executed in the db from your .NET code
* because EF is further abstracted from the db, it tends to be easier to abuse and introduce performance issues
* using stored procedures with Dapper puts all your sql in one place (the db) and makes it easier to manage
```csharp
// entity:
public List<Person> GetXPersonsEFCore(int personsNumber)
{
    return _context.Persons.Take(personsNumber).ToList();  
}
var personsRepository = new PersonsRepository(); 
personsRepository.GetXPersonsEFCore(1000);

// dapper
public List<Person> GetXPersonsDapper(int personsNumber)
{
    _dbConnection.Open();
    return _dbConnection.Query<Person>($"SELECT TOP({personsNumber}) * FROM Persons").ToList();
}
var personsRepository = new PersonsRepository();
personsRepository.GetXPersonsDapper(1000);

```
