The `getchar()` function in C is used to read a single character from the standard input (usually the keyboard). It is part of the C Standard I/O library and is defined in `stdio.h`. The function reads one character at a time and returns it as an `int`.

```c
#include <stdio.h>

int main() {
    int ch;

    printf("Enter a character: ");
    ch = getchar();  // Reads one character from input

    printf("You entered: %c\n", ch);
    return 0;
}


```

### How It Works:

1. **Input Handling**: The program will wait for the user to input a character and press "Enter." After pressing "Enter," the `getchar()` function reads the first character entered by the user.
2. **Return Value**: The character is returned as an `int`. This allows for checking if the returned value is `EOF` (typically used in reading files or handling errors).
3. **Multi-Character Input**: Even though `getchar()` reads only one character, if you type a full line of characters and press "Enter," the characters remain in the input buffer. Additional calls to `getchar()` will continue reading those characters one by one.
The `putchar()` function in C is used to output a single character to the standard output (typically the console). It is part of the C Standard I/O library and is defined in `stdio.h`. This function writes a character, passed as an argument, to the output stream.

```c
#include <stdio.h>

int main() {
    int ch1, ch2;

    printf("Enter two characters: ");
    ch1 = getchar();  // Reads the first character
    ch2 = getchar();  // Reads the second character

    printf("You entered: %c and %c\n", ch1, ch2);
    return 0;
}

```

# Example: Reading Until Newline (`\n`):

```c
#include <stdio.h>

int main() {
    int ch;

    printf("Enter a string (press Enter to finish): ");
    while ((ch = getchar()) != '\n') {  // Reads until Enter (newline)
        printf("You typed: %c\n", ch);
    }

    return 0;
}

```
#  Handling `EOF`:

`getchar()` returns `EOF` when the input ends or an error occurs. This is typically used in file reading or detecting the end of standard input (like `Ctrl + D` in Linux/Mac or `Ctrl + Z` in Windows).

### Example: Checking for `EOF`:
`getchar()` returns `EOF` when the input ends or an error occurs. This is typically used in file reading or detecting the end of standard input (like `Ctrl + D` in Linux/Mac or `Ctrl + Z` in Windows).

```c 
#include <stdio.h>

int main() {
    int ch;

    printf("Enter characters (Ctrl+D to end):\n");
    while ((ch = getchar()) != EOF) {
        printf("You typed: %c\n", ch);
    }

    return 0;
}

```

# Put char

```c
#include <stdio.h>

int main() {
    char ch = 'A';

    putchar(ch);  // Output the character 'A'
    putchar('\n');  // Output a newline character for formatting

    return 0;
}

```
- **`ch`**: This is the character to be written to the output. It's passed as an `int`, but it represents an `unsigned char` value.
- **Returns**: The function returns the character written as an `unsigned char` cast to `int`. If there’s an error, `EOF` is returned.
