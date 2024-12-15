Expression-bodied members provide a concise syntax for defining members (methods, properties, etc.) of a class or struct using a single expression. They use the => lambda like syntax

1 . Methods
```cs
class Example
{
    public int Add(int a, int b) => a + b;
}

```
2.Properties
```cs
class Example
{
    public int X { get; }
    public int Y { get; }

    public Example(int x, int y)
    {
        X = x;
        Y = y;
    }

    public int Sum => X + Y;
}

```
3. Constructors
```cs
class Example
{
    private string _name;

    public Example(string name) => _name = name;
}

```
