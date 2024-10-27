```c
int toupper(int ch);

```



EXAMPLE
```c
#include <stdio.h>
#include <ctype.h>

int main() {
    char ch = 'a';
    char upper = toupper(ch);  // Convert to uppercase

    printf("Original: %c, Uppercase: %c\n", ch, upper);  // Output: Original: a, Uppercase: A
    return 0;
}

```
#### Example 2
```c
#include <stdio.h>
#include <ctype.h>

void toUpperCase(char *str) {
    for (int i = 0; str[i] != '\0'; i++) {  // Loop until the end of the string
        str[i] = toupper(str[i]);  // Convert each character to uppercase
    }
}

int main() {
    char str[] = "Hello, World!";
    toUpperCase(str);  // Convert the string to uppercase

    printf("Uppercase String: %s\n", str);  // Output: UPPERCASE STRING: HELLO, WORLD!
    return 0;
}

```
