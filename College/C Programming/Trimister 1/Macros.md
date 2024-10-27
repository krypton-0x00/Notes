In C, **macros** are a feature of the preprocessor, allowing you to define constants, functions, or code snippets that are substituted before the actual compilation begins. Macros are often used to create more readable and maintainable code, especially for constants or code that is repeated in many places.

### Defining Macros:

Macros are defined using the `#define` directive.

### 1. **Object-like Macros (Constants):**

These macros are used to define constant values. When the macro is encountered, the preprocessor replaces the macro name with the defined value.

#### Syntax:

`#define MACRO_NAME value
`
```c
#include <stdio.h>

#define PI 3.14159

int main() {
    float radius = 5.0;
    float area = PI * radius * radius;
    
    printf("Area of circle: %.2f\n", area);
    return 0;
}

```
# 2. **Function-like Macros:**
`#define MACRO_NAME(arguments) expression
`

```c
#include <stdio.h>

#define SQUARE(x) ((x) * (x))

int main() {
    int num = 4;
    
    printf("Square of %d: %d\n", num, SQUARE(num));
    return 0;
}

```
# ### 3. **Multi-line Macros:**
Macros can span multiple lines by using the backslash (`\`) to indicate line continuation.
```c
#include <stdio.h>

#define MAX(a, b) \
    ({ __typeof__(a) _a = (a); \
        __typeof__(b) _b = (b); \
        _a > _b ? _a : _b; })

int main() {
    int x = 10, y = 20;
    
    printf("Maximum: %d\n", MAX(x, y));
    return 0;
}

```

# 4. **Conditional Compilation:**
You can use macros to include or exclude parts of the code based on certain conditions, which is useful for cross-platform code or debugging.

```c
#include <stdio.h>

#define DEBUG 1

int main() {
    int num = 10;
    
    #ifdef DEBUG
        printf("Debugging: num = %d\n", num);
    #endif
    
    return 0;
}

```
If you define `DEBUG` as 1, the `#ifdef` block is included during preprocessing. Otherwise, it's excluded from the final code.

### Common Predefined Macros:

- `__FILE__`: The name of the current source file.
- `__LINE__`: The current line number in the source file.
- `__DATE__`: The current date of compilation.
- `__TIME__`: The current time of compilation.
```c
#include <stdio.h>

int main() {
    printf("This code is in file: %s, line: %d\n", __FILE__, __LINE__);
    printf("Compiled on: %s at %s\n", __DATE__, __TIME__);
    return 0;
}

```
output
```yaml
This code is in file: example.c, line: 5
Compiled on: Sep 24 2024 at 10:30:00

```
 
# IFDEF 
```c
	#ifdef DEBUG
		printf("Debugging: num = %d\n", num);
	#endif
```
The `#ifdef` directive in C is a **preprocessor directive** that stands for **"if defined."** It checks whether a specific macro (in this case, `DEBUG`) has been defined earlier using `#define`. If the macro has been defined, the code within the `#ifdef` block is included and compiled. If the macro is not defined, the code is excluded from compilation.

```c
#ifdef MACRO_NAME
    // Code to be included if MACRO_NAME is defined
#endif

```
