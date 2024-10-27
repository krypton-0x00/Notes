Intuition 
if we have a

| Number | operation | Number    | result |
| ------ | --------- | --------- | ------ |
| +ve    | +         | +ve       | +ve    |
| -ve    | +         | +ve (big) | +ve    |
| +ve    | +         | -ve (big) | -ve    |
if we have a big -ve number sum with small +ve number we get a -ve number .

it states
if your subarray sum becomes -ve reset **it to zero** coz its better to add a zero then a -ve number

maxSum = 
currentSum = 

loop {
	over the array;
	 currentSum += arr[i];
	 if(currentSum < 0) currentSum = 0;
}![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-1.webp]]
![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-2.webp]]
![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-3.webp]]
![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-4.webp]]
![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-5.webp]]
![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-6.webp]]
![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-7.webp]]
```cpp

 
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum = INT_MIN;
        int currentSum = 0;
        for(int i = 0 ; i< nums.size(); i++){
            
            currentSum += nums[i];
            maxSum = max(currentSum,maxSum);
            if(currentSum < 0) currentSum = 0;
        }
        return maxSum;
    }
};

 

```

