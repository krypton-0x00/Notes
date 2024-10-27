The `goto` statement in C provides a way to jump to another point in the program. It's typically avoided in modern programming practices because it can make code harder to read and maintain, leading to what's called "spaghetti code." However, there are rare cases where `goto` can be useful, such as breaking out of deeply nested loops or handling errors in resource cleanup.



```c
#include <stdio.h>

int main() {
    int num = 5;

    if (num < 10) {
        goto small;
    } else {
        goto large;
    }

small:
    printf("Number is small.\n");
    return 0;

large:
    printf("Number is large.\n");
    return 0;
}

```


