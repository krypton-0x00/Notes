[189. Rotate Array](https://leetcode.com/problems/rotate-array/)

Medium

Topics

Companies

Hint

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example 1:**

**Input:** nums = [1,2,3,4,5,6,7], k = 3
**Output:** [5,6,7,1,2,3,4]
**Explanation:**
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

**Example 2:**

**Input:** nums = [-1,-100,3,99], k = 2
**Output:** [3,99,-1,-100]
**Explanation:** 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]


# SOL
### Approach: Reverse Technique

1. **Reverse the whole array**.
2. **Reverse the first `k` elements**.
3. **Reverse the remaining `n-k` elements**.

This approach efficiently rotates the array in place with a time complexity of **O(n)** and a space complexity of **O(1)**.

EG:
 [ 1,2,3,4,5] k=2
1. Reverse Whole Array:
[ 5, 4, 3, 2, 1]
2. Reverse the first K elems
[4,5,3,2,1]
3. Reverse the remaining n -k elements
[4,5,1,2,3]

```cpp
#include <iostream>
#include <vector>
#include <algorithm>  // For std::reverse

void reverseArray(std::vector<int>& nums, int start, int end) {
    while (start < end) {
        std::swap(nums[start], nums[end]);
        start++;
        end--;
    }
}

void rotate(std::vector<int>& nums, int k) {
    int n = nums.size();
    k = k % n;  // Handle cases where k >= n

    // Step 1: Reverse the whole array
    reverseArray(nums, 0, n - 1);

    // Step 2: Reverse the first k elements
    reverseArray(nums, 0, k - 1);

    // Step 3: Reverse the remaining n-k elements
    reverseArray(nums, k, n - 1);
}

int main() {
    std::vector<int> nums = {1, 2, 3, 4, 5, 6, 7};
    int k = 3;

    rotate(nums, k);

    for (int num : nums) {
        std::cout << num << " ";  // Output: 5 6 7 1 2 3 4
    }

    return 0;
}

```
### Why `k = k % n`?

Imagine you have an array of size `n`. Rotating the array by `n` or any multiple of `n` results in the array looking the same as it originally did. For example:

- If you have an array `[1, 2, 3, 4, 5]` (size `n = 5`), rotating it 5 times or any multiple of 5 (like 10, 15, etc.) brings it back to the original configuration.

So, if `k` is larger than `n`, rotating the array by `k` steps is equivalent to rotating it by `k % n` steps. This ensures that the number of steps is effectively reduced to something less than or equal to the size of the array.