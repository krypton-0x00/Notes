The `?:` operator in C is known as the **ternary operator** or **conditional operator**. It's a shorthand for `if-else` statements and allows you to return one of two values based on a condition.

```
condition ? expression_if_true : expression_if_false;

```

```c
#include <stdio.h>

int main() {
    int a = 10, b = 20;
    
    // Using ternary operator
    int max = (a > b) ? a : b;
    
    printf("Maximum value is: %d\n", max);
    return 0;
}

```