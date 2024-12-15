```cs
Type type = typeof(TypeName);

```

```cs
static void Main()
    {
        Type exampleType = typeof(Example);
	        Console.WriteLine(exampleType.FullName);
	         // Output: Namespace.Example
    }
```
# 2. **`nameof`**

`nameof` retrieves the name of a variable, type, or member as a `string`. It’s primarily used for refactor-safe code (if you rename the variable or member, the `nameof` result updates automatically).
```cs
class Program
{
    static void Main()
    {
        int myVariable = 10;
        Console.WriteLine(nameof(myVariable)); 
        // Output: myVariable
        Console.WriteLine(nameof(Main));
              // Output: Main
        Console.WriteLine(nameof(Program));
           // Output: Program
    }
}

```
### 3. **`sizeof`**

`sizeof` returns the size in bytes of a value type. It is typically used in unsafe code blocks or for low-level programming scenarios like working with memory directly.

```cs
class Program
{
    unsafe static void Main()
    {
        Console.WriteLine(sizeof(int));    
        // Output: 4 (bytes)
        Console.WriteLine(sizeof(double)); 
        // Output: 8 (bytes)
        Console.WriteLine(sizeof(char));   
        // Output: 2 (bytes)
    }
}

```

#### Notes

- `sizeof` works only for primitive types, structs, and enums.
- For non-primitive types, you should use `Marshal.SizeOf()` from the `System.Runtime.InteropServices` namespace.