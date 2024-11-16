```c
#include<stdio.h>
#include <stdlib.h>

void allocateMemory(){
   int *arr = (int*)malloc(5 * sizeof(int)); //alloc size of 5 ints.
   if(arr == NULL) return;
   //Resizing
   arr = (int*)realloc(arr, 10 * sizeof(int));
   if(arr == NULL) return;
   //freeing the memory
   free(arr);
   arr=NULL;
}
void contiguousAlloc(){
   int *arr = (int*)calloc(5, sizeof(int));
   if(arr == NULL)return;
   //free mem
   free(arr);
   arr = NULL;
}

int main(){
   int arr[5] = {1,2,3,4,5};
    int* ptr = arr;
    printf("%d",*(ptr + 1));
}
```

Sure! Let's break down `malloc`, `calloc`, `realloc`, and `free` to understand how each one works, where to use them, and common pitfalls.

---

### 1. **malloc (Memory Allocation)**
   - `malloc` stands for **memory allocation**. It allocates a block of memory of the specified size (in bytes) and returns a pointer to the beginning of the block.
   - The allocated memory is uninitialized, meaning it contains garbage values (whatever was previously in memory).
   - If the allocation fails (e.g., if there isn’t enough memory), `malloc` returns `NULL`, so it’s good practice to check if the pointer returned is `NULL`.

   **Syntax**:
   ```c
   void* malloc(size_t size);
   ```
   - `size_t` is the unsigned integer type used for sizes, typically defined in `<stddef.h>`.

   **Example**:
   ```c
   int *arr = (int*)malloc(5 * sizeof(int));  // Allocates memory for 5 integers
   if (arr == NULL) {
       printf("Memory allocation failed!\n");
       return 1;  // Exit if allocation fails
   }
   ```

   **Pitfalls**:
   - Forgetting to cast the return value (although optional in C, it’s good practice).
   - Not checking if the pointer returned is `NULL`.
   - Failing to free memory later, leading to memory leaks.

---

### 2. **calloc (Contiguous Allocation)**
   - `calloc` stands for **contiguous allocation**. It allocates memory similar to `malloc`, but it initializes all allocated bytes to zero.
   - It takes two arguments: the number of elements and the size of each element. It then multiplies them to determine the total memory needed.

   **Syntax**:
   ```c
   void* calloc(size_t num, size_t size);
   ```
   - `num` is the number of elements you need, and `size` is the size of each element.

   **Example**:
   ```c
   int *arr = (int*)calloc(5, sizeof(int));  // Allocates and zeroes memory for 5 integers
   if (arr == NULL) {
       printf("Memory allocation failed!\n");
       return 1;
   }
   ```

   **Difference from malloc**:
   - `malloc` leaves allocated memory uninitialized (garbage values), while `calloc` sets it to zero.
   - If you need memory set to zero, `calloc` is generally more efficient than manually zeroing memory after `malloc`.

---

### 3. **realloc (Reallocation)**
   - `realloc` is used to **resize previously allocated memory**. It can expand or shrink the existing memory block.
   - When expanding, `realloc` tries to extend the current block if possible. If there isn’t enough space, it allocates a new block, copies the existing data over, and frees the old memory.
   - When shrinking, `realloc` reduces the memory block but keeps the existing data intact up to the new size.
   - If the pointer passed to `realloc` is `NULL`, it behaves like `malloc`.

   **Syntax**:
   ```c
   void* realloc(void* ptr, size_t new_size);
   ```
   - `ptr` is the pointer to the memory block you want to resize, and `new_size` is the new desired size in bytes.

   **Example**:
   ```c
   int *arr = (int*)malloc(5 * sizeof(int));
   if (arr == NULL) {
       printf("Initial allocation failed!\n");
       return 1;
   }
   
   // Resizing to hold 10 integers
   arr = (int*)realloc(arr, 10 * sizeof(int));
   if (arr == NULL) {
       printf("Reallocation failed!\n");
       return 1;
   }
   ```

   **Pitfalls**:
   - Always check if `realloc` returns `NULL`—if it fails, the original memory remains allocated, but you may lose the pointer if not checked.
   - If you pass a `NULL` pointer to `realloc`, it behaves like `malloc`.
   - If you pass `0` as the size, it can act like `free`.

---

### 4. **free (Deallocation)**
   - `free` is used to **deallocate memory** previously allocated by `malloc`, `calloc`, or `realloc`. It releases the memory back to the operating system, making it available for future allocations.
   - **Important**: Failing to free memory can lead to memory leaks, which cause your program to consume more and more memory over time.

   **Syntax**:
   ```c
   void free(void* ptr);
   ```
   - `ptr` is a pointer to the memory block you want to free.

   **Example**:
   ```c
   free(arr);  // Frees the memory allocated for arr
   arr = NULL;  // Good practice to set the pointer to NULL after freeing
   ```

   **Pitfalls**:
   - Don’t call `free` on a pointer that wasn’t allocated by `malloc`, `calloc`, or `realloc`.
   - Don’t call `free` on a pointer more than once (double-free), as this leads to undefined behavior.
   - After calling `free`, it’s good practice to set the pointer to `NULL` to avoid dangling pointers.

---

### **Summary Table**

| Function | Purpose                | Arguments                 | Initial Values  |
|----------|-------------------------|---------------------------|-----------------|
| `malloc` | Allocates memory        | `size_t size`            | Uninitialized   |
| `calloc` | Allocates and zeros     | `size_t num, size_t size`| Zero-initialized|
| `realloc`| Resizes memory          | `void* ptr, size_t new_size` | Preserves data |
| `free`   | Deallocates memory      | `void* ptr`              | N/A             |

---

### **Example Scenario**

Let’s create a dynamic array that grows in size as more elements are added:

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *arr = (int*)malloc(2 * sizeof(int));  // Start with space for 2 elements
    if (arr == NULL) {
        printf("Initial allocation failed!\n");
        return 1;
    }

    int size = 2, count = 0;

    // Simulate adding elements
    for (int i = 0; i < 10; i++) {
        if (count == size) {  // If array is full, double its size
            size *= 2;
            int *temp = (int*)realloc(arr, size * sizeof(int));
            if (temp == NULL) {
                printf("Reallocation failed!\n");
                free(arr);  // Free old memory if realloc fails
                return 1;
            }
            arr = temp;
        }

        arr[count++] = i;  // Add element
    }

    // Print and free array
    for (int i = 0; i < count; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    free(arr);  // Free the allocated memory
    return 0;
}
```

This example starts with a small array, and whenever the array is full, it resizes (doubles) it using `realloc`. This is a common technique for building dynamic arrays.

Let me know if you'd like to dive into any specific part!