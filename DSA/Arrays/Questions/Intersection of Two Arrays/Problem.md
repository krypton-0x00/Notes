Given two integer arrays `nums1` and `nums2`, return _an array of their_ 

_intersection_

. Each element in the result must be **unique** and you may return the result in **any order**.

**Example 1:**

**Input:** nums1 = [1,2,2,1], nums2 = [2,2]
**Output:** [2]

**Example 2:**

**Input:** nums1 = [4,9,5], nums2 = [9,4,9,8,4]
**Output:** [9,4]
**Explanation:** [4,9] is also accepted.


```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int>set(nums1.begin(),nums1.end());
        unordered_set<int>result; //store the intersection 

        for(auto num : nums2){
            if(set.find(num) != set.end()){
                result.insert(num);
            }

        }
        return vector<int>(result.begin(),result.end());
         


        
 
    }
};
```
### Explanation:

1. **Set for `nums1`**: We use an `unordered_set` to store all unique elements of `nums1`.
2. **Iterate over `nums2`**: For each element in `nums2`, we check if it's present in the set created from `nums1`.
3. **Result Set**: We use another set `result` to store the intersection. This ensures the intersection itself doesn't have duplicates.
4. **Return as Vector**: Finally, the result set is converted into a vector to meet the return type requirement.
 
## Time complexity 
**O(n + m)**: where `n` is the size of `nums1` and `m` is the size of `nums2`. The `set` operations (insert and find) take constant time on average.