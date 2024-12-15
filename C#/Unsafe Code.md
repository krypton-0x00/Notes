### **Pointers in `unsafe` Code**
```cs
unsafe
{
    int x = 10;
    int* ptr = &x; // Pointer to the variable 'x'
    Console.WriteLine((int)ptr); // Memory address of 'x'
    Console.WriteLine(*ptr);     // Value at the address (10)
}

```
### **Fixed Keyword**

The `fixed` keyword is used to pin variables in memory, ensuring the Garbage Collector (GC) doesn’t move them.
```cs
unsafe
{
    int[] numbers = { 1, 2, 3 };
    fixed (int* ptr = numbers) // Pin the array in memory
    {
        for (int i = 0; i < numbers.Length; i++)
        {
            Console.WriteLine(*(ptr + i)); // Pointer arithmetic
        }
    }
}

```

### **Stack Allocation with `stackalloc`**

You can allocate memory on the stack instead of the heap using `stackalloc`. This is useful for scenarios requiring temporary arrays.

```cs
unsafe
{
    int* ptr = stackalloc int[3]; // Allocate an array of 3 integers on the stack
    ptr[0] = 10;
    ptr[1] = 20;
    ptr[2] = 30;

    for (int i = 0; i < 3; i++)
    {
        Console.WriteLine(ptr[i]);
    }
}

```

### **Pointers to Structs**

Pointers can also be used with structs. However, only types that are unmanaged (do not contain references to managed types) can be pointed to.

```cs
struct Point
{
    public int X, Y;
}

unsafe
{
    Point point = new Point { X = 10, Y = 20 };
    Point* ptr = &point;

    Console.WriteLine(ptr->X); // Access X using the arrow operator
    Console.WriteLine(ptr->Y); // Access Y using the arrow operator
}

```
### **Interop with Unmanaged Code**

Unsafe code is useful for calling unmanaged libraries (e.g., C DLLs).

```cs
[DllImport("user32.dll")]
public static extern int MessageBox(IntPtr hWnd, string text, string caption, int type);

unsafe
{
    MessageBox(IntPtr.Zero, "Hello, World!", "Test", 0);
}

```