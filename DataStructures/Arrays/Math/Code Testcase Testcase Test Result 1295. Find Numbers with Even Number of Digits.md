[1295. Find Numbers with Even Number of Digits](https://leetcode.com/problems/find-numbers-with-even-number-of-digits/)

Given an array `nums` of integers, return how many of them contain an **even number** of digits.

**Example 1:**

**Input:** nums = [12,345,2,6,7896]
**Output:** 2
**Explanation:** 
12 contains 2 digits (even number of digits). 
345 contains 3 digits (odd number of digits). 
2 contains 1 digit (odd number of digits). 
6 contains 1 digit (odd number of digits). 
7896 contains 4 digits (even number of digits). 
Therefore only 12 and 7896 contain an even number of digits.

**Example 2:**

**Input:** nums = [555,901,482,1771]
**Output:** 1 
**Explanation:** 
Only 1771 contains an even number of digits.

```cpp
int findNumbers(vector<int>& nums) {
        int even = 0;
        for(int i : nums){
            int digitCount = 0;
            while(i != 0){
                i = i / 10;
                digitCount++;
            }
            if(digitCount % 2 == 0) even++;
        }
        return even;
    }
```


# Optimized 
Only numbers between 10 and 99 or 1000 and 9999 or 100000 have even number of digits.  
Taking advantage of num <= 10^5 -> which is the constraint.
```cpp
class Solution {
public:
    int findNumbers(vector<int>& nums) {
        int even = 0;
        for(int i : nums){
            if( (10 <=i && i <= 99) || (1000<=i && i <= 9999) || (i == 100000)){
                even++;
            }
        }
        return even;
    }
};
```