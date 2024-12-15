In C#, `[something]` generally refers to an **attribute**, which is metadata applied to code elements like classes, methods, properties, or parameters. Attributes provide additional information to the compiler or runtime about the behavior or purpose of the element they decorate.

---

### **Attributes in C#**

Attributes are enclosed in square brackets (`[ ]`) and are used to add metadata. For example:

```csharp
[Serializable]
public class MyClass
{
    // This class can now be serialized
}
```

---

### **Common Uses of Attributes**

1. **Marking Classes for Serialization**
    
    ```csharp
    [Serializable]
    public class MyClass { }
    ```
    
2. **Testing**
    
    ```csharp
    [TestMethod]
    public void MyTest() { }
    ```
    
3. **Controlling Behavior**
    
    ```csharp
    [Obsolete("Use NewMethod instead.")]
    public void OldMethod() { }
    
    public void NewMethod() { }
    ```
    

---

### **Defining Custom Attributes**

You can create your own attributes by inheriting from `System.Attribute`.

#### Example:

```csharp
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method)]
public class MyCustomAttribute : Attribute
{
    public string Description { get; }
    public MyCustomAttribute(string description)
    {
        Description = description;
    }
}

// Using the custom attribute
[MyCustomAttribute("This is a custom attribute")]
public class MyClass { }
```

---

### **Attribute Usage**

1. **Decorating Classes or Methods**
    
    ```csharp
    [MyCustomAttribute("Example class")]
    public class Example { }
    ```
    
2. **Specifying Parameters**
    
    ```csharp
    public void MyMethod([Optional] string? name = null) { }
    ```
    
3. **Accessing Attributes via Reflection**
    
    ```csharp
    var attributes = typeof(MyClass).GetCustomAttributes(false);
    foreach (var attr in attributes)
    {
        Console.WriteLine(attr);
    }
    ```
    

---

### **Examples of Built-in Attributes**

1. **`[Obsolete]`** Marks a method or class as outdated.
    
    ```csharp
    [Obsolete("This method is deprecated.")]
    public void OldMethod() { }
    ```
    
2. **`[Serializable]`** Indicates that a class can be serialized.
    
    ```csharp
    [Serializable]
    public class Data { }
    ```
    
3. **`[DllImport]`** Allows calling unmanaged code.
    
    ```csharp
    [DllImport("user32.dll")]
    public static extern int MessageBox(IntPtr hWnd, string text, string caption, int type);
    ```
    
4. **`[TestMethod]`** Marks a method as a unit test in frameworks like MSTest.
    
    ```csharp
    [TestMethod]
    public void TestSomething() { }
    ```
    
5. **`[Required]`, `[Range]`, `[StringLength]`** Common in ASP.NET Core for data validation in models:
    
    ```csharp
    public class User
    {
        [Required]
        public string Name { get; set; }
    
        [Range(18, 100)]
        public int Age { get; set; }
    }
    ```
    

---

### **AttributeTargets Enum**

Attributes can be restricted to specific elements using `AttributeUsage` and the `AttributeTargets` enum.

#### Example:

```csharp
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method)]
public class MyCustomAttribute : Attribute { }
```

---

### **Summary**

- `[something]` in C# usually refers to an attribute.
- Attributes provide metadata to your code.
- Built-in attributes, like `[Obsolete]` and `[Serializable]`, are common and highly useful.
- You can create custom attributes for specific needs.
- Attributes can be accessed at runtime via **reflection**.