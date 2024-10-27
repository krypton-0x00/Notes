The reason array indexing starts from 0 in C (and in many other programming languages) is rooted in how memory addressing works at a low level. Here's a detailed explanation:

### 1. **Memory Addresses and Pointer Arithmetic**

When an array is created, it occupies a contiguous block of memory. The array name acts as a pointer to the first element, i.e., the address of the first element is the base address of the array.

For example, consider the array:
`int arr[5] = {10, 20, 30, 40, 50};

`
- `arr[0]` refers to the first element (`10`) stored at the base address `arr`.
- `arr[1]` refers to the second element (`20`) stored at an offset of 1 from the base address.
- `arr[2]` refers to the third element (`30`) stored at an offset of 2, and so on.

Internally, C calculates the memory address of an array element using pointer arithmetic:

```c
Address of arr[i] = Base address + (i * size_of_element)

```
Where:

- `Base address` is the address of the first element, i.e., `arr`.
- `i` is the index of the element.
- `size_of_element` is the size (in bytes) of the type of the array element.
# if
Now, if indexing started at 1, the formula would become:
```c
Address of arr[i] = Base address + ((i-1) * size_of_element)

```
This extra subtraction would require more computation, slowing things down. Starting indexing at 0 eliminates this unnecessary subtraction, simplifying the calculation to:

# Details
### 1. **Current C Array Indexing (0-Based)**

In C, arrays are **0-indexed**, meaning the first element is at index `0`. So, if you access `arr[0]`, you're accessing the first element, and the formula for calculating its address is straightforward:

`Address of arr[i] = Base address + (i * size_of_element)`

- `arr[0]` corresponds to:
    
   
    
    `Address = Base address + (0 * size_of_element) = Base address`
    
- `arr[1]` corresponds to:
    
    
    
    `Address = Base address + (1 * size_of_element)`
    

This makes sense because `arr[0]` is stored at the base address, and `arr[1]` is stored right after it, with a memory offset of one element size.

# 2. **If Array Indexing Started from 1**
Now, if C had **1-based indexing** (like in some other languages), the first element of the array would be `arr[1]`, not `arr[0]`. This would mean that the index of the first element is `1`, but the memory address still starts from the base address.

To calculate the address of `arr[i]`, you would need to "shift" the index to account for the fact that the array starts at 1 instead of 0. So the address formula would become:

```c
Address of arr[i] = Base address + ((i - 1) * size_of_element)

```
Here’s why:

- For `arr[1]` (the first element in 1-based indexing):
```c
Address = Base address + ((1 - 1) * size_of_element) = Base address
This works because the first element should be at the base address.
```
For `arr[2]` (the second element in 1-based indexing):
```c
Address = Base address + ((2 - 1) * size_of_element) = Base address + (1 * size_of_element)

```