Just promises that the data will not change ; but remember we can break the promise
#Tip 
 people having trouble remembering the order in which const keyword is to be used, here's a quick tip.
You have to read it backward, like the compiler does. For instance : 

1. const int * A; ==> "A is a pointer to an int that is constant."
	 OR
2. int const* A; ==> "A is a pointer to a const int"\
Both are same
	 
```cpp
#include <iostream>
using namespace std;

int main() {
    const int x = 10;
    // x = 20; // Error: x is a constant and cannot be modified

    cout << "x = " << x << endl;
    return 0;
}


```

**Pointer to constant data**: The data pointed to by the pointer cannot be changed, but the pointer itself can be modified to point to another location.
```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;
    int b = 20;
    const int* ptr = &a;  // Data is constant, pointer is not

    // *ptr = 15; // Error: cannot modify the value pointed to by ptr
    ptr = &b;     // Pointer can change to point to another variable

    cout << "ptr points to: " << *ptr << endl;
    return 0;
}

```
**Constant pointer**: The pointer itself is constant and cannot point to any other location, but the data it points to can be modified.
Example: Constant pointer
```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;
    int b = 20;
    int* const ptr = &a;  // Pointer is constant, data is not

    *ptr = 15;    // OK: data can be modified
    // ptr = &b;  // Error: pointer cannot point to another address

    cout << "a = " << a << endl;
    return 0;
}

```
**Constant pointer to constant data**: Both the pointer and the data it points to are constant and cannot be modified.
```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;
    const int* const ptr = &a;  // Both pointer and data are constant

    // *ptr = 15; // Error: cannot modify the value
    // ptr = &b;  // Error: cannot change the pointer

    cout << "ptr points to: " << *ptr << endl;
    return 0;
}

```
# **`const` Member Functions**
A member function can be declared as `const`, which means it will not modify any member variables of the object (except for mutable ones). This is useful when you want to define "read-only" methods that do not alter the state of the object.
```cpp
#include <iostream>
using namespace std;

class MyClass {
    int value;
public:
    MyClass(int v) : value(v) {}

    int getValue() const {  // const member function
        // value = 20; // Error: cannot modify member variable
        return value;
    }
};

int main() {
    MyClass obj(10);
    cout << "Value: " << obj.getValue() << endl;  // OK: getValue is const
    return 0;
}

```
### Summary of `const` Usage:

- **Constant variables**: The value cannot be changed after initialization.
- **Pointers**:
    - `const int* ptr`: Pointer to constant data.
    - `int* const ptr`: Constant pointer to modifiable data.
    - `const int* const ptr`: Constant pointer to constant data.
- **Function parameters**: Ensures arguments cannot be modified within the function.
- **References**: `const int& ref` ensures the referenced value cannot be modified.
- **Member functions**: `void func() const` guarantees the function will not modify the object.
- **Return types**: Prevents modification of the returned value by the caller.
- **Class members**: Constant class members must be initialized in the constructor and cannot be changed afterward.