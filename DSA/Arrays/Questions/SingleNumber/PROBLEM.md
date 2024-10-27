Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

 

Example 1:

Input: nums = [2,2,1]
Output: 1
Example 2:

Input: nums = [4,1,2,1,2]
Output: 4
Example 3:

Input: nums = [1]
Output: 1


# We know
1. Xor operator
2. if we do 0 xor 0 = 0
3. 1 xor 1 = 0 
4. 1 xor 0 = 1
5. 0 xor 1 = 1
6. i.e xor cancels out the same inputs.
###### Also
n xor 0 = n
n xor n = 0
1 xor 0 = 1
5 xor 0 = 5;

Input: nums = [2,2,1]

## solution
2 xor 2 xor 1
0010  xor 0010 = 0000 xor 0001 => 0001 => 1
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int target = nums[0];
        for(int i = 1 ; i<nums.size();i++){
            target = target ^ nums[i];
        }
        return target;
    }
};
```
Better Syntax
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int target = 0;
        for(int val : nums){
            target ^= val;
        }
        return target;
    }
};
```