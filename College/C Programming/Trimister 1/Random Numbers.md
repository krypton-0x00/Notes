```c
#include<stdlib.h>
printf(rand()); //generates a random number from 0 to RAND_MAX ie (2^31 - 1)
```
Lets run about code 3 times
we get Output:
```
12312312
12312312
12312312
```
# Why 

The Rand(void) returns pseudo-random numbers in the range of - to RAND_MAX
The random number is determined byy the initial seed value and by default seed value is 1.

so since the seed value is same `rand(1)` all random nos. will be same.

# Change seed value
`srand()`
```c
srand(100) //set seed
rand(); //generate random

```

# FIX
so generate a random seed everytime we can use time :} 
```c
#include<time.h>
#include<stdlib.h>

time_t time(time_t *seconds);  //prototype of the functions
//Returns time since 00:00:00 UTC , jan 1 , 1970 (unix timestamp) in seconds;


```
```c 
#include <stdio.h>
#include <stdlib.h> // For rand(), srand()
#include <time.h>   // For time()

int main() {
    // Seed the random number generator with the current time
    srand(time(0));

    // Generate and print 5 random numbers
    for (int i = 0; i < 5; i++) {
        int random_number = rand();  // Generates a random number
        printf("Random Number %d: %d\n", i + 1, random_number);
    }

    return 0;
}

```
```output
Random Number 1: 12345
Random Number 2: 6789
Random Number 3: 23456
Random Number 4: 9876
Random Number 5: 34567

```
### 3. **Generating Random Numbers in a Range**

To generate random numbers within a specific range, such as between `min` and `max`, you can use the following formula:
`rand()->0 - 2^31` % (max - min + 1)
100000 % (6 - 0 + 1);

```c
int random_number = rand() % (max - min + 1) + min;

```
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    srand(time(0));  // Seed the random number generator

    int min = 1, max = 100;
    
    // Generate and print 5 random numbers between min and max
    for (int i = 0; i < 5; i++) {
        int random_number = rand() % (max - min + 1) + min;
        printf("Random Number %d: %d\n", i + 1, random_number);
    }

    return 0;
}

```