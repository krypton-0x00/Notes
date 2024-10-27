[88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

Solved

Easy

Topics

Companies

Hint

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be _stored inside the array_ `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example 1:**

**Input:** nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
**Output:** [1,2,2,3,5,6]
**Explanation:** The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.



# ### Approach:

1. **Three Pointers**:
    
    - One pointer (`p1`) for the last element in the first `m` elements of `nums1`.
    - One pointer (`p2`) for the last element in `nums2`.
    - One pointer (`p`) for the last index of `nums1` (i.e., `m + n - 1`).
2. **Compare and Merge**:
    
    - Start from the end of both arrays, compare the elements, and place the larger one at the end of `nums1`.
3. **Remaining Elements**:
    
    - If there are any remaining elements in `nums2`, copy them into `nums1` (there's no need to check `nums1` as it's already in place).
```cpp
#include <iostream>
#include <vector>

void merge(std::vector<int>& nums1, int m, std::vector<int>& nums2, int n) {
    int p1 = m - 1;  // Pointer for nums1
    int p2 = n - 1;  // Pointer for nums2
    int p = m + n - 1;  // Pointer for the last position in nums1

    while (p1 >= 0 && p2 >= 0) {
        if (nums1[p1] > nums2[p2]) {
            nums1[p] = nums1[p1];
            p1--;
        } else {
            nums1[p] = nums2[p2];
            p2--;
        }
        p--;
    }

    // If there are remaining elements in nums2, copy them to nums1
    while (p2 >= 0) {
        nums1[p] = nums2[p2];
        p2--;
        p--;
    }
}

int main() {
    std::vector<int> nums1 = {1, 2, 3, 0, 0, 0};
    int m = 3;
    std::vector<int> nums2 = {2, 5, 6};
    int n = 3;

    merge(nums1, m, nums2, n);

    for (int num : nums1) {
        std::cout << num << " ";  // Output: 1 2 2 3 5 6
    }

    return 0;
}

```
### Explanation:

1. **Pointers Setup**: `p1` is initialized to the last valid element of `nums1` (`m - 1`), `p2` to the last element of `nums2` (`n - 1`), and `p` to the last index of `nums1` (`m + n - 1`).
2. **Merge in Reverse**: We compare `nums1[p1]` and `nums2[p2]`, placing the larger element at `nums1[p]` and moving the corresponding pointer (`p1` or `p2`).
3. **Remaining Elements in nums2**: After the main loop, any remaining elements from `nums2` (if `p2` is still non-negative) are copied into `nums1`.

### Time Complexity:

- **O(m + n)**: We traverse both arrays exactly once.

 