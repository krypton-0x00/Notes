Introduced in C# 9.0, **records** are a lightweight and immutable type primarily used for storing data. They provide concise syntax, built-in immutability, and value-based equality (compared by contents, not references).

**Immutability** By default, all properties in a record are immutable (`init`-only).
```cs
public record Person(string FirstName, string LastName);

var person = new Person("John", "Doe");
// person.FirstName = "Jane"; // Error: Properties are immutable

```
**Value-Based Equality** Records compare values instead of references:

```cs
var person1 = new Person("John", "Doe");
var person2 = new Person("John", "Doe");
Console.WriteLine(person1 == person2); // Output: True

```

**Concise Syntax** Records can be declared in a single line:
```cs
public record Person(string FirstName, string LastName);

```
**`with` Expressions** Allows creating a copy of a record with some modified properties:
```cs
var person1 = new Person("John", "Doe");
var person2 = person1 with { FirstName = "Jane" };
Console.WriteLine(person2); // Output: Person { FirstName = Jane, LastName = Doe }

```

**Deconstruction** Records support deconstruction for extracting values:

```cs
var person = new Person("John", "Doe");
var (firstName, lastName) = person;
Console.WriteLine($"{firstName} {lastName}"); // Output: John Doe

```

# ADVANCE

1. Supports Inheritance
```cs
public record Employee(string FirstName, string LastName, string Position) : Person(FirstName, LastName);

```
2. You can define methods in records
```cs
public record Person(string FirstName, string LastName)
{
    public string FullName => $"{FirstName} {LastName}";
}

```