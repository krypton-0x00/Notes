`gcc -o combined main.c part1.c part2.c`



```c
// math_functions.h


#ifndef MATH_FUNCTIONS_H
#define MATH_FUNCTIONS_H

// Function declarations
int add(int a, int b);
int subtract(int a, int b);

#endif

```


```c
// math_functions.c


#include "math_functions.h"

// Function definitions
int add(int a, int b) {
    return a + b;
}

int subtract(int a, int b) {
    return a - b;
}

```
```c
// main.c


#include <stdio.h>
#include "math_functions.h"

int main() {
    int x = 10, y = 5;

    printf("Add: %d\n", add(x, y));           // Calls add() from math_functions.c
    printf("Subtract: %d\n", subtract(x, y)); // Calls subtract() from math_functions.c

    return 0;
}

```
`$ gcc -o combine main.c math_functions.c`

OR We can only compile 

``
- **Compile each `.c` file into an object file**:
    
    `gcc -c main.c math_functions.o`
    
    The `-c` option tells the compiler to generate object files (`.o`) without linking.
    
- **Link the object files to create the final executable**:
    `gcc main.o math_functions.o -o my_program`
    
    This command links `main.o` and `math_functions.o` to produce the executable `my_program`.
    