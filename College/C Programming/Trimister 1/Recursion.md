# 1.Factorial of a number

The factorial of a number `n` is defined as:

n!=n×(n−1)×(n−2)×…×1n! = n \times (n-1) \times (n-2) \times \ldots \times 1n!=n×(n−1)×(n−2)×…×1

Using recursion, this can be represented as:

n!=n×(n−1)!n! = n \times (n-1)!n!=n×(n−1)!

Where the base case is `0! = 1`.
```c
#include <stdio.h>

// Function to calculate factorial using recursion
int factorial(int n) {
    if (n == 0)  // Base case: 0! = 1
        return 1;
    else         // Recursive case: n * factorial(n-1)
        return n * factorial(n - 1);
}

int main() {
    int num = 5;
    printf("Factorial of %d is %d\n", num, factorial(num));
    return 0;
}


```
### Explanation:

- When `factorial(5)` is called:
    1. `factorial(5)` calls `factorial(4)`
    2. `factorial(4)` calls `factorial(3)`
    3. `factorial(3)` calls `factorial(2)`
    4. `factorial(2)` calls `factorial(1)`
    5. `factorial(1)` calls `factorial(0)`
    6. `factorial(0)` returns `1`, and the function starts returning the computed values in reverse order.

Thus, `factorial(5)` eventually returns `5 * 4 * 3 * 2 * 1 = 120`.

# 2.The **handshake problem** is a classic combinatorial problem that asks:

**"If there are `n` people in a room, and each person shakes hands with every other person exactly once, how many handshakes occur?"**
 
 ![[Pasted image 20241009171554.png]]



```cpp

int handshake(int n) {
    if (n <= 1) return 0;  // Base case: 0 or 1 person -> 0 handshakes
    return (n - 1) + handshake(n - 1);  // Recursive case
}



```