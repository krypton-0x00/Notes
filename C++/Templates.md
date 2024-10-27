**Templates** in C++ are a powerful feature that allows you to write **generic** and **flexible** code. With templates, you can create functions and classes that can operate with any data type, making the code reusable and reducing redundancy.

There are two main types of templates in C++:

1. **Function Templates**
2. **Class Templates**
### 1. Function Templates

A **function template** allows you to create a single function that can work with different types of data. Instead of writing multiple overloaded functions for different data types, you define a template, and the compiler generates the appropriate function based on the type passed to it.

#### Example:
```cpp
#include <iostream>
using namespace std;

template <typename T>  // 'T' is a placeholder for a data type
T add(T a, T b) {
    return a + b;
}

int main() {
    cout << add<int>(3, 4) << endl;      // Output: 7 (int)
    cout << add<double>(3.5, 2.2) << endl;  // Output: 5.7 (double)
    return 0;
}

```
In the example:

- `T` is a **template parameter** (a placeholder for a type).
- The `add()` function works for any type `T` as long as the `+` operator is valid for that type.
- When you call `add<int>(3, 4)`, the compiler substitutes `int` for `T`.

#### Template Argument Deduction:

You can often let the compiler automatically deduce the template type from the arguments:

```cpp
cout << add(3, 4) << endl;       // Output: 7 (compiler infers 'int')
cout << add(3.5, 2.2) << endl;   // Output: 5.7 (compiler infers 'double')

```

### 2. Class Templates

A **class template** allows you to define a generic class. The template allows the class to work with any data type, making it more reusable and flexible.

#### Example:

```cpp
#include <iostream>
using namespace std;

template <typename T>
class Box {
    T value;
public:
    Box(T v) : value(v) {}

    T getValue() {
        return value;
    }
};

int main() {
    Box<int> intBox(100);      // Box that stores an int
    Box<double> doubleBox(99.99);  // Box that stores a double

    cout << "intBox: " << intBox.getValue() << endl;  // Output: 100
    cout << "doubleBox: " << doubleBox.getValue() << endl;  // Output: 99.99

    return 0;
}

```
In the example:

- `Box<T>` is a class template, where `T` is a placeholder for a data type.
- The `Box<int>` object is instantiated for `int`, and `Box<double>` for `double`.

### 3. Template Specialization

Sometimes, you might need a specialized version of a template for a specific type. In such cases, you can use **template specialization**.

#### Example:

```cpp
#include <iostream>
using namespace std;

template <typename T>
class Box {
public:
    Box(T v) {
        cout << "Generic template" << endl;
    }
};

// Specialization for char type
template <>
class Box<char> {
public:
    Box(char v) {
        cout << "Specialized template for char" << endl;
    }
};

int main() {
    Box<int> intBox(100);   // Generic template
    Box<char> charBox('A'); // Specialized template for char

    return 0;
}

```
In the example:

- The specialized version of `Box` for `char` is provided separately from the generic version.
- When `Box<char>` is used, the specialized version is chosen.

### where will we use Template Specialization
### 1. **Handling Different Data Types Differently**

Sometimes, you want to use a generic template for most types, but a specific type (like `char`, `bool`, or `float`) might require special handling.

#### Example: Specializing for `char` Type

```cpp
#include <iostream>
using namespace std;

template <typename T>
void printValue(T value) {
    cout << "Generic Template: " << value << endl;
}

// Specialization for char
template <>
void printValue<char>(char value) {
    cout << "Specialized Template for char: " << value << endl;
}

int main() {
    printValue(42);       // Generic Template
    printValue(3.14);     // Generic Template
    printValue('A');      // Specialized Template for char

    return 0;
}

```



### 4. Template Parameters

In addition to **type parameters**, templates can also accept **non-type parameters**, like integers or pointers. This allows for more flexible template designs.

#### Example:
```cpp
#include <iostream>
using namespace std;

template <typename T, int size>
class Array {
    T arr[size];  // Fixed-size array
public:
    void set(int index, T value) {
        arr[index] = value;
    }

    T get(int index) {
        return arr[index];
    }
};

int main() {
    Array<int, 5> intArray;  // Array of 5 integers
    intArray.set(0, 100);

    cout << intArray.get(0) << endl;  // Output: 100

    return 0;
}

```
In this example:

- The template accepts a **non-type parameter** `int size`, specifying the size of the array.
- The array size is fixed at compile time.

### 5. Variadic Templates

C++11 introduced **variadic templates**, which allow a template to accept an arbitrary number of parameters. This is particularly useful for writing functions like `printf` that accept a variable number of arguments.

#### Example:
```cpp
#include <iostream>
using namespace std;

template <typename T>
void print(T value) {
    cout << value << endl;
}

template <typename T, typename... Args>
void print(T first, Args... args) {
    cout << first << " ";
    print(args...);  // Recursively call with remaining arguments
}

int main() {
    print(1, 2.5, "Hello", 'A');  // Output: 1 2.5 Hello A

    return 0;
}

```

### Summary of Templates

|Feature|Description|
|---|---|
|**Function Templates**|Allow functions to work with any type.|
|**Class Templates**|Allow classes to work with any type.|
|**Template Specialization**|Allows customizing templates for specific data types.|
|**Template Parameters**|Can accept both types and non-types (like integers).|
|**Variadic Templates**|Allow templates to accept an arbitrary number of arguments.|