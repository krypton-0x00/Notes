

A **static constructor** initializes static data or performs actions that need to be done once per type, such as setting up static fields. Static constructors are parameterless and are executed automatically before the type is accessed for the first time.

```cs
class MyClass
{
    public static int Counter;

    // Static constructor
    static MyClass()
    {
        Console.WriteLine("Static constructor called.");
        Counter = 42; // Initialize static data
    }
}

class Program
{
    static void Main()
    {
        Console.WriteLine(MyClass.Counter); // Triggers the static constructor
        Console.WriteLine(MyClass.Counter); // Static constructor is not called again
    }
}

```