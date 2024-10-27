In C, **type conversion** refers to converting a value from one data type to another. This can happen in two ways:

1. **Implicit Type Conversion (Type Coercion)**: The compiler automatically converts one data type to another without the programmer’s intervention.
2. **Explicit Type Conversion (Type Casting)**: The programmer manually converts a data type to another by specifying the target type.

# Implicit 
```c
#include <stdio.h>

int main() {
    int a = 10;
    float b = 2.5;
    float result = a + b;  // `a` is implicitly converted to `float`

    printf("Result: %f\n", result);  // Output: 12.500000
    return 0;
}

```

```c
int main() {
    int x = 5.7;  // `5.7` is a `float`, but it is implicitly converted to `int`
    printf("x: %d\n", x);  // Output: 5 (decimal part is truncated)
    return 0;
}

```

# 2. **Explicit Type Conversion (Type Casting)**

`(type) expression
`

```c
#include <stdio.h>

int main() {
    int a = 10;
    float b = 3.0;
    float result = (float)a / b;  // Cast `a` to `float` before division

    printf("Result: %f\n", result);  // Output: 3.333333
    return 0;
}

```
## casting between types in assignments
```c
#include <stdio.h>

int main() {
    float f = 3.14;
    int x = (int)f;  // Explicitly cast `float` to `int`

    printf("Float: %f, Cast to int: %d\n", f, x);  // Output: Float: 3.140000, Cast to int: 3
    return 0;
}

```
### Differences Between Coercion and Casting:

|**Aspect**|**Type Coercion** (Implicit)|**Type Casting** (Explicit)|
|---|---|---|
|**Who controls it?**|The compiler automatically decides.|The programmer explicitly controls it.|
|**Safety**|Generally safe, but can cause precision or range issues.|May cause precision loss or other issues if not done carefully.|
|**When used?**|In mixed-type operations or assignments.|When the programmer wants to force a conversion for specific needs.|
|**Example**|`int x = 10.5;` (implicit cast to `int`)|`int x = (int) 10.5;` (explicit cast)|
