{1,2,2,1,1}

question says the frequency of the number should be more then the n/2 in the array.
ie: element should exits on more then the half of array;
i.e There can only be 1 majority element;


[1, 2, 2, 1, 1,] here n = 5
# Brute force
Get frequency

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        for(int i = 0 ; i< nums.size(); i++){
            int freq = 0;
            for(int j = 0 ; j < nums.size(); j++){
                if(nums[i]==nums[j]){
                    freq++;
                }
            }
            if(freq > (nums.size()/2)){
                return nums[i];
            }
        }
        return 0;
    }
};
```
better way using sorting

```cpp
int majorityElement(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int freq = 1;
        int ans = nums[0];
        if(nums.size() == 1) return nums[0];
        for(int i = 1 ; i< nums.size(); i++){
            if(nums[i] == nums[i-1]){
                freq++;
            }
            if(freq>(nums.size()/2)){
                return nums[i];
            }
            
        }
        return freq;
    }
```

# Moore's Algorithm O(n)
count the frequency of the elements and set the answer
if the element is same frequency++;
if the elme is not same freq--;

coz if we do freq-- the majority elements frequency will still be higher.
```cpp
int majorityElement(vector<int>& nums) {
        
        int freq = 0;
        int ans = 0;
        for(int i = 0 ; i< nums.size(); i++){
             if(freq == 0){
                ans = nums[i];
             }
             //if we are at the same element : freq++ else: freq--;
             if(ans == nums[i])freq++;
             else freq--;
        }
        return ans;
    }
```