[283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)

Easy

Topics

Companies

Hint

Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

**Example 1:**

**Input:** nums = [0,1,0,3,12]
**Output:** [1,3,12,0,0]

**Example 2:**

**Input:** nums = [0]
**Output:** [0]

### Approach:

1. **Two Pointers**:
    
    - One pointer (`nonZeroIndex`) keeps track of the position where the next non-zero element should go.
    - Iterate through the array and, for each non-zero element, swap it with the element at the `nonZeroIndex` and then increment the pointer.
2. **Maintain Order**:
    
    - By using this approach, the relative order of the non-zero elements is maintained, and the zeroes are naturally pushed to the end.
-
```cpp
#include <iostream>
#include <vector>

void moveZeroes(std::vector<int>& nums) {
    int nonZeroIndex = 0;  // Keeps track of where the next non-zero element should go

    // Move all non-zero elements to the front of the array
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] != 0) {
            std::swap(nums[nonZeroIndex], nums[i]);
            nonZeroIndex++;
        }
    }
}

int main() {
    std::vector<int> nums = {0, 1, 0, 3, 12};

    moveZeroes(nums);

    for (int num : nums) {
        std::cout << num << " ";  // Output: 1 3 12 0 0
    }

    return 0;
}

```

### Explanation:

1. **`nonZeroIndex`**: This pointer keeps track of where the next non-zero element should be placed.
2. **Swapping**: If a non-zero element is found during iteration, it's swapped with the element at the `nonZeroIndex`. This ensures that non-zero elements are shifted to the front while maintaining their relative order.
3. **Zeros Automatically Move**: Once all non-zero elements are moved, the remaining positions in the array will contain zeroes.

### Time Complexity:

- **O(n)**: We traverse the array once.