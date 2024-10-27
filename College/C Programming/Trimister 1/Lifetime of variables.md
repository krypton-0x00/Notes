**Automatic Variables (Local Variables):**

- By default, variables declared within a block (without the `static` keyword) are **automatic variables**.
- Their lifetime is limited to the execution of the block in which they are defined. Once the block is exited, the memory for these variables is deallocated.
- Example
- 
```c
void myFunction() {
    int x = 10;  // x's lifetime is during the execution of myFunction
}
// x no longer exists after the function ends

```
**Static Variables:**

- A variable declared with the `static` keyword, either inside a function or outside it, has a **lifetime throughout the entire execution of the program**.
- If declared inside a function, it retains its value between function calls.
- If declared outside a function, it behaves like a global variable but is restricted to the file scope.
- Example


```c
void myFunction() {
    static int count = 0;  // count persists across function calls
    count++;
    printf("%d\n", count);
}

```
**Global Variables:**

- Variables declared outside of any function have a **lifetime** that spans the entire execution of the program. They are initialized when the program starts and deallocated when the program ends.

```c
int y = 20;  // Global variable, exists until program termination

```
**Dynamic Variables:**

- Variables allocated using `malloc()`, `calloc()`, or `realloc()` from the heap have **dynamic lifetimes**.
- These variables exist until they are explicitly deallocated using `free()`.
```c
int *ptr = (int *)malloc(sizeof(int));  // dynamically allocated
free(ptr);  // deallocation is manual

```
# Storage Class
In C programming, a **storage class** defines the scope, lifetime, visibility, and default initial value of variables or functions. C provides several storage class specifiers to control how memory is allocated for a variable and how the variable can be accessed within the program.

There are four main storage classes in C:

### 1. **Automatic Storage Class (`auto`)**

- **Scope:** Block scope (local to the function or block in which it's declared).
- **Lifetime:** The variable exists until the block or function exits (automatic lifetime).
- **Default Initial Value:** Indeterminate (garbage value) unless explicitly initialized.
- **Usage:** This is the default storage class for local variables declared inside functions or blocks.
- **Keyword:** `auto` (optional, rarely used as it’s the default for local variables).

Example:
```c
void myFunction() {
    auto int x = 10;  // `auto` is optional here
}

```
### 2. **External Storage Class (`extern`)**

- **Scope:** File scope (global visibility, accessible throughout the program).
- **Lifetime:** Exists from the start to the end of the program (global lifetime).
- **Default Initial Value:** Zero (for uninitialized variables).
- **Usage:** The `extern` keyword is used to declare a global variable or function that is defined in another file, or to refer to a variable that is defined elsewhere in the same file.
- **Keyword:** `extern`

Example (for declaring a variable defined in another file):

```cpp
// File 1: main.c
extern int count;  // Declaration of a variable defined elsewhere
void main() {
    printf("%d", count);  // Access the external variable
}

// File 2: global.c
int count = 10;  // Definition of the external variable

```
### 4. **Register Storage Class (`register`)**

- **Scope:** Block scope (local to the function or block where it's declared).
- **Lifetime:** The variable exists until the block or function exits (automatic lifetime).
- **Default Initial Value:** Indeterminate (garbage value) unless explicitly initialized.
- **Usage:** The `register` keyword suggests that the variable be stored in a CPU register (if available), which allows for faster access than storing in RAM. This is typically used for frequently accessed variables like loop counters.
- **Keyword:** `register`

**Note:** The use of `register` is advisory, meaning the compiler may ignore this suggestion if there are no available CPU registers or if it optimizes the variable differently.

Example:
```c
void myFunction() {
    register int i;  // Suggests to store i in a CPU register
    for (i = 0; i < 10; i++) {
        printf("%d", i);
    }
}

```
### Summary of Storage Classes

|**Storage Class**|**Scope**|**Lifetime**|**Default Initial Value**|**Keyword**|
|---|---|---|---|---|
|**auto**|Block (local to function)|Automatic (until block ends)|Indeterminate (garbage)|`auto` (optional)|
|**extern**|File (global across files)|Static (program lifetime)|Zero (for uninitialized)|`extern`|
|**static**|Block or file|Static (program lifetime)|Zero (for uninitialized)|`static`|
|**register**|Block (local to function)|Automatic (until block ends)|Indeterminate (garbage)|`register`|