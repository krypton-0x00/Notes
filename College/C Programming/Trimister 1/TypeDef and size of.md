### 1. **`typedef` in C**

`typedef` is a keyword in C that is used to create an alias or new name for an existing data type. It doesn't create a new type, but instead provides a way to simplify complex type declarations or make code more readable.

#### Syntax:
```c
typedef existing_type new_name;

```
- **`existing_type`**: The type you want to give a new name to.
- **`new_name`**: The alias or new name for the type.

#### Use Cases of `typedef`:

1. **Simplifying complex type declarations**.
2. **Making code more readable** by using more descriptive type names.
3. **Abstracting types**, which is useful in large programs where the underlying types might change but you don’t want to modify the entire codebase.

#### Example 1: Using `typedef` for basic types
```c
#include <stdio.h>

typedef unsigned long ulong;

int main() {
    ulong a = 100000;  // `ulong` is an alias for `unsigned long`
    printf("Value: %lu\n", a);
    return 0;
}

```
#### Example 2: Using `typedef` with structs

Without `typedef`, declaring a `struct` looks like this:

```c
struct Point {
    int x, y;
};

int main() {
    struct Point p1;
    p1.x = 10;
    p1.y = 20;
    return 0;
}

```
WITH 'typedef'
```c
typedef struct {
    int x, y;
} Point;  // `Point` is now an alias for `struct {int x, y;}`

int main() {
    Point p1;  // No need to write `struct` anymore
    p1.x = 10;
    p1.y = 20;
    return 0;
}

```
# 2. **`sizeof` Operator in C**
```c
#include <stdio.h>

int main() {
    printf("Size of int: %zu bytes\n", sizeof(int));  // %zu is used to print size_t
    printf("Size of char: %zu bytes\n", sizeof(char));
    printf("Size of float: %zu bytes\n", sizeof(float));
    return 0;
}

```
```yaml
Size of int: 4 bytes
Size of char: 1 byte
Size of float: 4 bytes

```
### With arrays
```c
#include <stdio.h>

int main() {
    int arr[10];

    printf("Size of array: %zu bytes\n", sizeof(arr));           // Size of the entire array
    printf("Size of one element: %zu bytes\n", sizeof(arr[0]));  // Size of one element
    printf("Number of elements: %zu\n", sizeof(arr) / sizeof(arr[0]));  // Total elements in the array

    return 0;
}

```


```yaml 
Size of array: 40 bytes
Size of one element: 4 bytes
Number of elements: 10

```
# With Structs
```c
#include <stdio.h>

typedef struct {
    int x;  //4
    char y; //4
    double z; //8
} MyStruct;

int main() {
    printf("Size of struct: %zu bytes\n", sizeof(MyStruct));
    return 0;
}

#output
Size of struct: 16 bytes


```