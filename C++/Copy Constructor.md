### 1. **Copy (Shallow vs. Deep)**

- **Shallow Copy**:
    
    - This is when you copy an object, but instead of duplicating the entire object and its associated data, only a reference or pointer to the same memory location is copied.
    - In this case, two objects end up sharing the same data, which can lead to issues like **aliasing**. Changing the data in one object will affect the other object.
    
    Example in C++:
```cpp
int a = 5;
int* p1 = &a;  // p1 points to the memory address of a
int* p2 = p1;  // p2 is a shallow copy of p1, both point to the same memory address

```
# 2 **Deep Copy**:

- In deep copying, the entire object, including all dynamically allocated memory, is duplicated. The two objects are completely independent.
- This ensures that changes to one object do not affect the other.

Example in C++:
```cpp
int* p1 = new int(5);  // Dynamically allocate memory for an integer
int* p2 = new int(*p1);  // Deep copy: p2 points to a new memory location with the same value

```


# Copy constructor
A **copy constructor** is a special constructor that defines how to create a new object as a copy of an existing object a. It is invoked when: 

- An object is initialized from another object of the same class.
- An object is passed by value to a function.
- An object is returned by value from a function.

The copy constructor takes a reference to the original object as its parameter, allowing it to make a copy of the object.

# The OG example

Basic but there is an issue the issue is that the rates of the book is a pointer i.e common  to both
so after the code is finished 
Destructor will attempt to delete Book1 and after that it will do it again but the pointer which was common is already deleted. 
## this leads to double free error;

```cpp
#include <iostream>
using namespace std;

class Book {
public:
    string Title;
    string Author;
    float* Rates;
    int RatesCounter;

    Book(string title, string author) { ... }
    
    ~Book() {
        delete[] Rates;
        Rates = nullptr;
    }

    Book(const Book& original) {
        Title = original.Title;
        Author = original.Author;
        Rates = original.Rates;
        RatesCounter = original.RatesCounter;
    }
};
```
# TO fix this

```cpp

#include <iostream>
using namespace std;

class Book {
public:
    string Title;
    string Author;
    float* Rates;
    int RatesCounter;

    Book(string title, string author) { ... }
    
    ~Book() {
        delete[] Rates;
        Rates = nullptr;
    }

    Book(const Book& original) {
        Title = original.Title;
        Author = original.Author;
        RatesCounter = original.RatesCounter;
        Rates  = new float[RatesCounter];
		for(int i = 0 ; i < RatesCounter; i++){
			//copy all the rates from the original object
			Rates[i] = original.Rates[i];
		}
    }
};


```

```cpp
class MyClass {
    int* data;  // Pointer to dynamically allocated memory

public:
    MyClass(int value) {
        data = new int(value);  // Constructor: allocate memory
    }

    // Copy constructor for deep copy
    MyClass(const MyClass &other) {
        data = new int(*other.data);  // Allocate new memory and copy value
    }

    ~MyClass() {
        delete data;  // Destructor: free allocated memory
    }
};

```

Example 2
```cpp
#include <iostream>

class ArrayClass {
    int* array;
    int size;

public:
    // Constructor
    ArrayClass(int s) : size(s) {
        array = new int[size];
        for (int i = 0; i < size; i++) {
            array[i] = i + 1;
        }
        std::cout << "Constructor called. Array created." << std::endl;
    }

    // Copy constructor (Deep Copy)
    ArrayClass(const ArrayClass& other) {
        size = other.size;
        array = new int[size];
        for (int i = 0; i < size; i++) {
            array[i] = other.array[i];
        }
        std::cout << "Copy Constructor called. Array copied." << std::endl;
    }

    // Destructor
    ~ArrayClass() {
        delete[] array;
        std::cout << "Destructor called. Array deleted." << std::endl;
    }

    void printArray() const {
        for (int i = 0; i < size; i++) {
            std::cout << array[i] << " ";
        }
        std::cout << std::endl;
    }
};

int main() {
    ArrayClass arr1(5);  // Constructor is called
    ArrayClass arr2 = arr1;  // Copy constructor is called (deep copy)

    arr1.printArray();
    arr2.printArray();

    return 0;
}

```