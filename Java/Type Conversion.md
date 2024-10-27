### 1. **Implicit Type Conversion (Widening)**

**Implicit type conversion** happens automatically when a smaller data type is converted to a larger data type without any data loss. Java handles this type of conversion for you, as it is considered safe.

#### Conversion Hierarchy:
```arduino
byte → short → int → long → float → double
```
### 2. **Explicit Type Conversion (Narrowing or Casting)**

**Explicit type conversion** (or **casting**) is required when you're trying to convert a larger data type to a smaller one, which can potentially lead to data loss. You have to tell Java explicitly to perform the conversion because it may result in loss of precision or overflow.

To do this, you use a cast operator `(type)` before the value.
```java
public class ExplicitConversion {
    public static void main(String[] args) {
        double num = 10.5;  // Double type
        int result = (int) num;  // Explicit conversion from double to int (casting)
        
        System.out.println("Double value: " + num);       // Output: 10.5
        System.out.println("Converted int value: " + result);  // Output: 10
    }
}

```
### 3. **Type Conversion Between Primitive Types and Objects (Boxing and Unboxing)**

In Java, there are **wrapper classes** for every primitive type. These wrapper classes allow you to convert primitives into objects and vice versa.

#### Wrapper Classes:

- `int` → `Integer`
- `double` → `Double`
- `char` → `Character`
- `boolean` → `Boolean`
- ... and so on.

Java provides **automatic** conversion between primitive types and their corresponding wrapper objects. This process is called **autoboxing** (primitive to object) and **unboxing** (object to primitive).

#### Autoboxing Example:
```java
public class AutoboxingExample {
    public static void main(String[] args) {
        int num = 50;
        Integer obj = num;  // Autoboxing: int to Integer (primitive to object)
        
        System.out.println("Primitive int value: " + num);    // Output: 50
        System.out.println("Wrapped Integer object: " + obj);  // Output: 50
    }
}

```
